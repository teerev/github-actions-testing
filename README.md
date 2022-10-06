# github-actions-testing

Instructions for setting up automated CI/CD pipeline

## Instructions:


1. create new github repo ````myrepo````  on github site

2. clone repo locally and navigate into directory
````bash
git clone myrepo;
cd myrepo
````

3. create venv locally
````bash
python3 -m venv .myvenv
````

4. activate venv
````bash
source .myvenv/bin/activate
````

5. install flask and pytest
````bash
pip install flask pytest
````

6. put requirements in txt
````bash
pip freeze > requirements.txt
````

7. make directory for source code and navigate into it
````bash
mkdir src;
cd src
````

8. write python file main.py, with something like the following

````python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():a
    return "Hello, world!"

if __name__ == '__main__':
    app.run()
````

9. make directory for tests and navigate to it
````bash
mkdir tests
cd ../tests
````

10. write python file test_app.py with following contents
````bash
from app import index

def test_index():
    assert index() == 'Hello, world!"
````

11. add src directory to PYTHONPATH
````bash
export PYTHONPATH=src
````

12. write .gitignore file with following contents
````bash
.venv/
__pycache__/
````

13. git add / commit / push
````bash
git add .
git commit -m "initial commit"
git push -u origin main
````

14. on github actions page, select Python application option

15. use the following .yml file to test code (github saves this as myrepo/.github/workflows/<filename>.yml)

````yml
# This workflow will install Python dependencies, run tests and lint with a single version of Python
# For more information see: https://help.github.com/actions/language-and-framework-guides/using-python-with-github-actions

name: Python application

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  contents: read

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python 3.10
      uses: actions/setup-python@v3
      with:
        python-version: "3.10"
	
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install flake8 pytest
        if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
	
    - name: Lint with flake8
      run: |
        # stop the build if there are Python syntax errors or undefined names
        flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
        # exit-zero treats all errors as warnings. The GitHub editor is 127 chars wide
        flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics
	
    - name: Test with pytest
      run: |
        export PYTHONPATH=src
	pip install pytest
        pytest
````

16. sign up to heroku on heroku.com

17. install heroku cli
````bash
sudo snap install --classic heroku
````

18. log in to heroku from cli (browser window pops up - click login)
````bash
heroku login
````

19. create new heroku project and note url  (e.g. https://limitless-brook-60765.herokuapp.com  |  https://git.heroku.com/limitless-brook-60765.git)
````bash
heroku create
````

20. create new heroku authorization and note down the 'Token:' entry
````bash
heroku authorizations:create
````

21. on myrepo github page, go to "Settings" ---> "Secrets", click "Add a new secret". Give it name "HEROKU_API_TOKEN" and paste the token into the "Value" field

22. Click "Add a new secret" again, and give this one name "HEROKU_APP_NAME" and paste the app name (e.g.  "limitless-brook-60765")  into the "Value" field

23. add the automated deployment action to the .yml file in myrepo/.github/workflows/<filename>.yml

````yml
    - name: Deploy to Heroku
      env:
        HEROKU_API_TOKEN: ${{ secrets.HEROKU_API_TOKEN}}
        HEROKU_APP_NAME: ${{ secrets.HEROKU_APP_NAME}}
      if: github.ref == 'refs/heads/main' && job.status == 'success'
      run: |
        git remote add heroku https://heroku:$HEROKU_API_TOKENA@git.heroku.com/$HEROKU_APP_NAME.git
        git push heroku HEAD:master -f
````






















