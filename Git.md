# Solo Workflow
## Check for remotes
git remote -v

## Removing Git
rm -rf .git

# Team Workflow
## 0. Estimating
  - view, models, admin
  -  

## 1. Pull from main
`git checkout main`
`git pull origin main`
`pip install -r requirements.txt`

## 2. Create Feature Branch
`git checkout -b` `123-feature-name`

## 2. Fetch and Switch to feature
`git fetch`
`git checkout 123-feature-name`

## 3. DB setup
`createdb sis`
`cd project/`
`python manage.py migrate`
`python manage.py createsuperuser`
`python manage.py collectstatic --no-input`

## 4. Push DB changes
`python manage.py makemigrations` `model_name`
`python manage.py migrate` `model_name`

## 5. Run Server
`python manage.py runserver`

## 6. Run Tests
- coverage: `./coverage`  or `coverage run --source='.' manage.py test myapp`
- all:      `python3 manage.py test --keepdb --settings=sis.settings.testing`
- app:      `python3 manage.py test --keepdb --settings=sis.settings.testing` `app_name`
- Specific: `python3 manage.py test --keepdb --settings=sis.settings.testing` `app_name.tests.test_name`

## 7. Committing
`git status`
`git add` `individual_file_name`
`git commit -m` `message`
`git push origin` `123-feature-name`

# General Git Commands
- git init
- git add file_name
- git commit -m "message"
- git checkout -b NAME_OF_NEW_ BRANCH - make and switch to a new branch
- git checkout NAME_OF_BRANCH - switch to a branch
- git branch -a - see all of your branches (including remote ones)
 - git branch -D NAME_OF_BRANCH - delete a branch
 - git merge NAME_OF_BRANCH_TO_MERGE
 - git remote add NAME_OF_REMOTE NAME_OF_REPO_URL
 - git push NAME_OF_REMOTE NAME_OF_LOCAL_BRANCH
 - git pull NAME_OF_REMOTE NAME_OF_BRANCH
 - git stash v.s. git stash pop
 - git fetch
 - git reset HEAD~1
 - git commit --amend

------------------------