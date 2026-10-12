# Python Project Deployment 

## 1. Introduction
This project demonstrates deployment of a Python Flask application on AWS EC2 using GitHub and Gunicorn.

## 2. Architecture Diagram


<img width="1536" height="1024" alt="Flask on AWS EC2 Deployment Flowchart (1)" src="https://github.com/user-attachments/assets/82e4ca45-92e1-46b6-8785-117e87e3deda" />


## 3. Input
- HTTP request to `/`
- HTTP request to `/hi`

## 4. Output

<img width="1919" height="1075" alt="Screenshot 2026-10-10 174556" src="https://github.com/user-attachments/assets/90b82a4d-61ff-49f3-89f5-347b660f8adc" />

## 5. Installation

Update the system and install Python and Git:

```bash
sudo yum update -y
sudo yum install python3 git -y
```

Clone the GitHub repository:

```bash
git clone <YOUR-REPOSITORY-URL>
cd <YOUR-REPOSITORY-NAME>
```

Create and activate virtual environment:

```bash
python3 -m venv myenv
source myenv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## 6. Run Application

```bash
python app.py
```

## 7. Background Run – Gunicorn

```bash
gunicorn --bind 0.0.0.0:5000 app:app --daemon
```

Check the application in your browser:

```text
http://<EC2-PUBLIC-IP>:5000/
http://<EC2-PUBLIC-IP>:5000/hi
```

**Note:** Allow TCP port `5000` in the EC2 Security Group.

## 8. Conclusion

Successfully deployed a Python Flask application on AWS EC2 using GitHub and Gunicorn.

