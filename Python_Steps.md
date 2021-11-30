### 1. Database Schema (models.py)
```py
"""SQLAlchemy models for Friender."""
from flask_sqlalchemy import SQLAlchemy
from flask_bcrypt import Bcrypt

bcrypt=Bcrypt()
db=SQLAlchemy()


class User(db.Model):
    __tablename__ = 'users'

    user_id = db.Column(
        db.Integer,
        primary_key=True,
    )
    username = db.Column(
        db.Text,
        nullable=False,
        unique=True,
    )
    password = db.Column(
        db.Text,
        nullable=False,
    )

    @classmethod #to allow us to easily return JSON Objects of the database data
    def serialize(cls, self):
        """Serialize to dictionary"""
        return {
            "username": self.username,
            "password": self.password,
        }

    @classmethod
    def signup(cls, username):
        """
        Sign up user.
        Hashes password and adds user to system.
        """
        hashed_pwd = bcrypt.generate_password_hash(password).decode('UTF-8')

        user = User(
            username=username,
            password=hashed_pwd,
        )

        db.session.add(user)
        db.session.commit()
        return user



def connect_db(app):
    """
    Connect this database to provided Flask app.
    """
    db.app = app
    db.init_app(app)
```

### 2. Seed Data (seed.py)
```py
"""Seed database with sample data."""
from app import db
from models import User

db.drop_all() #drops all tables
db.create_all() #creates all tables

initialUser = User(username="initial_user", password="password")

db.session.add(initialUser)
db.session.commit()

#psql db_name -f file_name.sql
```

### 3. AWS Server set up
https://s3.console.aws.amazon.com/
set permissions to allow public reading of urls

### 4. Environmental Variables
```env
AWS_ACCESS_KEY_ID=Secret
AWS_SECRET_ACCESS_KEY=Really_Secret
DATABASE_URL=postgresql:///database_name
BUCKET=bucket.name
```

### 5. GIT Ignore File
```gitignore
venv/
.env
__pycache__
```

### 6. Unsecured CRUD Routes (app.py)
```py
import os

from flask import Flask, jsonify, request, session
from flask_debugtoolbar import DebugToolbarExtension
from werkzeug.utils import secure_filename
from flask_cors import CORS
import boto3
import dotenv

from models import db, connect_db, User

dotenv.load_dotenv()

CURR_USER_KEY = "curr_user"
BASE_PHOTO_URL="https://s3.us-west-1.amazonaws.com/friender.davis.colin/"

app = Flask(__name__)
CORS(app) #Turning CORS off

bcrypt= Bcrypt()

# Get DB_URI from environ variable (useful for production/testing) or,
# if not set there, use development local db.
app.config['SQLALCHEMY_DATABASE_URI'] = os.environ['DATABASE_URL']
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
app.config['SQLALCHEMY_ECHO'] = False
app.config['DEBUG_TB_INTERCEPT_REDIRECTS'] = True
app.config['SECRET_KEY'] = os.environ['SECRET_KEY']
toolbar = DebugToolbarExtension(app)
# toolbar = DebugToolbarExtension(app)

connect_db(app)

s3 = boto3.client( #setting up aws sdk
  "s3",
  "us-west-1",
  aws_access_key_id=os.environ['AWS_ACCESS_KEY_ID'],
  aws_secret_access_key=os.environ['AWS_SECRET_ACCESS_KEY'],
)

##############################################################################
# User Routes
@app.get('/users')
def list_users():
    """Returns JSON list of all user dictionaries.
        {users:
            [{user1}, ...]
        }
    """

    users = User.query.all()
    serialized = [User.serialize(user) for user in users]

    return jsonify(users=serialized)

@app.get('/users/<int:user_id>')
def get_user(user_id):
    """
    Show user data.
    {
        "username": data,        
    }
    """

    user = User.query.get_or_404(user_id)
    serialize = User.serialize(user)

    return jsonify(user=serialize)



@app.post('/users')
def create_user():
    """
    Takes is user info as dictionary:
        {
            "username": u_data,        
        }
    Adds a new user to the database, and returns added user object.
    """
    img = request.files['file']
    if img:
        filename = secure_filename(img.filename)
        s3.upload_fileobj(
            img, 
            os.environ['BUCKET'], 
            filename, 
            ExtraArgs={"ACL":"public-read"}
        )

    user = User.signup(
            username=request.form["username"],
            image=filename,
            password=request.form["password"],
        )
    serialized = User.serialize(user)
    return jsonify(user=serialized)

@app.patch('/users/<int:user_id>')
def edit_user(user_id):
    """Updates a user. Reutrns updated user.
    {
        "username": "u_data",      
    }
    """
    user = User.query.get_or_404(user_id)
    newData = request.json

    for key, value in newData.items():
        setattr(user,key,value)
    
    db.session.add(user)
    db.session.commit()
    updated_user = User.query.get_or_404(user_id)
    serialize = User.serialize(updated_user)

    return jsonify(user=serialize)

@app.delete('/users/<int:user_id>')
def delete_user(user_id):
    """
    Deletes a user. 
    Returns { "deleted": "user_id" }
    """
    user = User.query.get_or_404(user_id)

    db.session.delete(user)
    db.session.commit()

    return ({"deleted":user_id})
```

### 7. API (api.ts)
```ts
import axios from "axios";


const BASE_API_URL = "http://localhost:5000";

//FIXME: Update with jobly api
/** Bluerprint for making request 
 * accepts endpoint for request, data, method(default to get)
 * returns response wrapped in try/catch
*/

class FrienderApi {
  /**Registers user in database. 
   * Takes in registration formData:
    {
        "username",
        "password"        
    }
   * Output: registered user
   *
    {
        "username",
        "password"        
    }
    */
    static async registerUser(formData) {
        const result = await axios.post(
            `${BASE_API_URL}/users`, 
            formData, 
            { headers: {'Content-Type': 'multipart/form-data'}
        });
        
        return result.data;
    }
    /**Gets user by user_id
     * Input: userId  
     * Output: user data LIKE:
     *{ 
         "user": 
        { "username" } 
    }*/
    static async getById(userId) {
    const result = await axios.get(`${BASE_API_URL}/users/${userId}`);
    return result.data;
    }

  /**deletes user based upon id
  * Input: userId  
  * Output: {"deleted": userId}*/
  static async deleteById(userId) {
    const result = await axios.delete(`${BASE_API_URL}/users/${userId}`);
    return result.data;
  }

  /**deletes user based upon id
  * Input: userId and newUserData LIKE {key,value} e.g. 
  * {username: 'newUsername',
  * hobbies:'new hobbies'
  * } 
  * Output: updated user data object {user:{username,first_name,......}}*/
  static async updateUser(userId,newUserData) {
    const result = await axios.patch(`${BASE_API_URL}/users/${userId}`, newUserData);
    return result.data;
  }

  /**get all users in database
  * Input: nothing
  * Output: {users:{user},{user},{user},......}*/
     static async getAll() {
      const result = await axios.get(`${BASE_API_URL}/users`);
      return result.data;
    }
}

export default FrienderApi;
```

## Python Non-Sever Side Code------------------
### DocTests
```py
def bounded_avg(nums):
    """Return avg of nums (makes sure nums are 1-100)

       >>> bounded_avg([1, 2, 3])
       2

       >>> bounded_avg([1, 2, 101])
       Traceback (most recent call last):
           ...
       ValueError: Outside bounds of 1-100
    """

    for n in nums:
        if n < 1 or n > 100:
            raise ValueError("Outside bounds of 1-100")

    return sum(nums) / len(nums)

    #Run Test: python3 -m doctest -v your-file.py
```

### Classes
```py
class Triangle:
    """Right triangle."""

    def __init__(self, a, b):
        """"Create triangle from a and b sides."""
        self.a = a
        self.b = b

    def __repr__(self):
    return f"<Triangle a={self.a} b={self.b}>"

    def describe(self):
        """Return description of area."""
        return f"My area is {self.get_area()}"
#####################
class ColoredTriangle(Triangle):
    """Triangle that has a color."""

    def __init__(self, a, b, color):
        # get parent class [`super()`], call its `__init__()`
        super().__init__(a, b)

        self.color = color

    def describe(self):
        """Return description of area and color."""
        msg = super().describe() + f" I am {self.color}"

```

### START HERE: Stopped at Flask Intro