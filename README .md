# Python Flask Application Deployment on AWS EC2

## 1. Introduction

This project is a simple Python web application developed using **Flask** and deployed on an **AWS EC2 Linux instance**. The source code is stored in GitHub and can be cloned onto the EC2 instance for deployment.

The application runs on port **5000** and provides two routes:

- `/` — Displays a deployment success message.
- `/hi` — Displays a greeting message from Flask and Docker.

### Technologies Used

- Python 3
- Flask
- Git and GitHub
- AWS EC2
- Gunicorn
- PM2 (optional, for process management)

## 2. Architectural Diagram

```text
Developer
    |
    | Push source code
    v
GitHub Repository
    |
    | git clone
    v
AWS EC2 Linux Instance
    |
    | Install Python dependencies
    v
Python Flask Application (app.py)
    |
    | Runs on port 5000
    v
Gunicorn / PM2 Process Management
    |
    v
Browser
http://<EC2-PUBLIC-IP>:5000/
```

## 3. Input

The application accepts HTTP requests through these routes:

| Endpoint | Input | Response |
|---|---|---|
| `/` | Browser HTTP request | Deployment success message |
| `/hi` | Browser HTTP request | HTML greeting message |

No form input or database input is required for the current version of this application.

## 4. Output

After deployment, open the following URLs in a browser. Replace `<EC2-PUBLIC-IP>` with your EC2 instance's public IPv4 address.

- **Home route:** `http://<EC2-PUBLIC-IP>:5000/`
- **Greeting route:** `http://<EC2-PUBLIC-IP>:5000/hi`

Expected output:

- `/` displays: `successfully deployed python application through jenkins!!!!!!!!!, added webhook`
- `/hi` displays a heading with a greeting from Flask and Docker.

## 5. Deployment Steps

### Step 1: Connect to EC2

Connect to your Linux EC2 instance using SSH.

### Step 2: Install required packages

For Amazon Linux versions that support `yum`:

```bash
sudo yum update -y
sudo yum install python3 git -y
```

### Step 3: Clone the GitHub repository

```bash
git clone <YOUR-REPOSITORY-URL>
cd <YOUR-REPOSITORY-NAME>
```

Replace the placeholders with your repository URL and folder name.

### Step 4: Create and activate a virtual environment

```bash
python3 -m venv myenv
source myenv/bin/activate
```

### Step 5: Install dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### Step 6: Test the application

```bash
python app.py
```

Open port `5000` in the EC2 security group's inbound rules. For testing, restrict the source to your own public IP when possible.

Visit:

```text
http://<EC2-PUBLIC-IP>:5000/
http://<EC2-PUBLIC-IP>:5000/hi
```

Stop the test server with `Ctrl+C`.

## 6. Background Run — PM2

PM2 can manage a Python process if Python is installed and available as `python3`.

Install PM2 on the instance. This requires Node.js and npm to be installed first:

```bash
sudo npm install -g pm2
```

From the project directory, with the virtual environment activated, start the application:

```bash
pm2 start app.py --name python-flask-app --interpreter python3
```

Useful PM2 commands:

```bash
pm2 status
pm2 logs python-flask-app
pm2 restart python-flask-app
pm2 stop python-flask-app
pm2 delete python-flask-app
pm2 save
```

To configure PM2 processes to return after a reboot, follow the `pm2 startup` command shown by PM2 for your operating system, then run `pm2 save`.

**Alternative — Gunicorn:** Gunicorn is included in `requirements.txt`. You can run the Flask app in the background with:

```bash
gunicorn --bind 0.0.0.0:5000 app:app --daemon
```

Use either PM2 or Gunicorn for the chosen process-management approach; avoid starting multiple copies that try to use the same port.

## 7. AWS Security Group

Ensure the EC2 instance's attached security group has an inbound rule for:

- Type: Custom TCP
- Port: `5000`
- Source: Your IP address for testing, or another appropriately restricted range

For a production deployment, consider a reverse proxy such as Nginx and HTTPS rather than exposing the application port publicly.

## 8. Conclusion

This project demonstrates how to clone a Python Flask application from GitHub, install its dependencies, and deploy it on an AWS EC2 instance. The application exposes two routes on port `5000`. PM2 or Gunicorn can be used to manage the running process.

## Author

**Your Name**

## License

Add a license if you plan to distribute this project publicly.
