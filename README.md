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

8. write python file main.py with following contents

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

15. use the template .yml file:

````yml
testasd;flkj;
- 
````