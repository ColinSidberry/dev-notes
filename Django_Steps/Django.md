## 1. Create Project
- django-admin startproject mysite

### 2. Create App
- python manage.py startapp polls

### 3. Create Database & Default Tables
- psql 
- CREATE DATABASE db_name
- python manage.py migrate

### 4. Create Models
- Change your models (in models.py).
- python manage.py makemigrations app_name
- python manage.py migrate 

### 5. Create Admin
- python manage.py createsuperuser

### 6. Run Server
- python manage.py runserver

### 7. Run Tests
- python manage.py test app_name


## Project Sections--------------------------------
1. models - database schema
2. templates
3. url - routes
4. views - what's shown/done on a specific route
5. apps - application name?
6. admin - admin way to control the db
7. test



## Showing Latest User Comment

### 1. models.py
added the new model
### 2. templates
create html where the template needs to be shown
### 3. View (??why did the user comments show without any alterations to the view??)
UserProfileView for some reason doesn't need to be updated in order for us to see the new UserComment info

## Showing All User Comments

### template: Create the new template

### view: create the new view (?? don't understand the query ??)

### urls: connect the view and the template

### admin: (__foreign keys??)

### Create factory 
??when do we use faker??, 
??what does meta do?? 
??when do we use SubFactory vs. RelatedFactory??
??When do we use lazy??

### test_models.py-->Start Here

### test_views.py
