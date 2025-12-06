# Online Quiz System

A comprehensive Django-based online examination platform that enables teachers to create courses and quizzes, while students can take exams and track their performance.

![Python](https://img.shields.io/badge/Python-3.7.6+-blue.svg)
![Django](https://img.shields.io/badge/Django-3.0.5-green.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

---

## Table of Contents
- [Features](#features)
- [System Architecture](#system-architecture)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [User Roles](#user-roles)
- [Database Schema](#database-schema)
- [Screenshots](#screenshots)
- [Known Issues](#known-issues)
- [Contributing](#contributing)
- [License](#license)

---

## Features

### Core Functionality
- **Multi-role System**: Admin, Teacher, and Student roles with distinct permissions
- **Course Management**: Create and manage multiple courses/exams
- **Question Bank**: Add MCQ questions with 4 options and configurable marks
- **Exam System**: Students can take unlimited exam attempts
- **Result Tracking**: View detailed marks for each exam attempt
- **Dashboard Analytics**: Real-time statistics for all user roles
- **Contact Form**: Email-based contact system

### Security Features
- User authentication and authorization
- Role-based access control
- Teacher approval workflow
- Secure password validation

---

## System Architecture

### Technology Stack
- **Backend**: Django 3.0.5
- **Database**: SQLite3 (default)
- **Frontend**: HTML, CSS, JavaScript with Django Templates
- **UI Enhancement**: django-widget-tweaks

### Project Structure
```
onlinequiz/
├── onlinequiz/          # Main project configuration
│   ├── settings.py      # Project settings
│   ├── urls.py          # URL routing
│   └── wsgi.py          # WSGI configuration
├── quiz/                # Quiz app (courses, questions, results)
│   ├── models.py        # Course, Question, Result models
│   ├── views.py         # Quiz logic
│   └── forms.py         # Quiz forms
├── teacher/             # Teacher app
│   ├── models.py        # Teacher model
│   ├── views.py         # Teacher functionality
│   └── urls.py          # Teacher routes
├── student/             # Student app
│   ├── models.py        # Student model
│   ├── views.py         # Student functionality
│   └── urls.py          # Student routes
├── templates/           # HTML templates
├── static/              # Static files (CSS, JS, images)
├── manage.py            # Django management script
└── requirements.txt     # Python dependencies
```

---

## Installation

### Prerequisites
- Python 3.7.6 or higher
- pip (Python package manager)
- Git (optional)

### Step-by-Step Installation

1. **Clone or Download the Project**
   ```bash
   git clone <repository-url>
   cd onlinequiz-master
   ```

2. **Create Virtual Environment (Recommended)**
   ```bash
   python -m venv venv
   
   # On Windows
   venv\Scripts\activate
   
   # On macOS/Linux
   source venv/bin/activate
   ```

3. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run Database Migrations**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

5. **Create Superuser (Admin)**
   ```bash
   python manage.py createsuperuser
   ```
   Follow the prompts to set username, email, and password.

6. **Start Development Server**
   ```bash
   python manage.py runserver
   ```

7. **Access the Application**
   Open your browser and navigate to:
   ```
   http://127.0.0.1:8000/
   ```

---

## Configuration

### Email Settings (Contact Form)

To enable the contact form functionality, update the following settings in `onlinequiz/settings.py`:

```python
EMAIL_HOST_USER = 'your-email@gmail.com'
EMAIL_HOST_PASSWORD = 'your-app-password'
EMAIL_RECEIVING_USER = ['recipient@gmail.com']
```

**Important for Gmail Users:**
- Use an App Password instead of your regular password
- Enable 2-Factor Authentication on your Gmail account
- Generate an App Password: [Google Account Settings](https://myaccount.google.com/apppasswords)

### Database Configuration

By default, the project uses SQLite. To use PostgreSQL or MySQL, update the `DATABASES` setting in `settings.py`.

---

## Usage

### Admin Panel
Access at: `http://127.0.0.1:8000/admin`

**Capabilities:**
- View dashboard with system statistics
- Manage teachers (approve, update, delete)
- Manage students (view, update, delete)
- Create and manage courses/exams
- Add questions to courses with marks
- View student results and performance

### Teacher Portal

**Registration & Login:**
1. Apply for teacher account
2. Wait for admin approval
3. Login after approval

**Capabilities:**
- View dashboard with course and question statistics
- Create and manage courses
- Add MCQ questions to courses
- Delete questions
- View course details

### Student Portal

**Registration & Login:**
1. Sign up (no approval required)
2. Login immediately after registration

**Capabilities:**
- View available courses and exams
- Take exams (unlimited attempts)
- View marks for each exam attempt
- Track performance history

---

## User Roles

### 1. Administrator
- **Access**: Full system control
- **Key Functions**:
  - User management (teachers and students)
  - Course and exam creation
  - Question bank management
  - Result monitoring
  - Teacher approval workflow

### 2. Teacher
- **Access**: Course and question management
- **Key Functions**:
  - Create courses/exams
  - Add and manage questions
  - View system statistics
- **Note**: Requires admin approval before login

### 3. Student
- **Access**: Exam taking and result viewing
- **Key Functions**:
  - Browse available courses
  - Take exams multiple times
  - View detailed results
  - Track performance

---

## Database Schema

### Models Overview

**Course Model**
- `course_name`: Course/exam name
- `question_number`: Number of questions
- `total_marks`: Total marks for the course

**Question Model**
- `course`: Foreign key to Course
- `marks`: Marks for the question
- `question`: Question text
- `option1-4`: Four answer options
- `answer`: Correct answer (Option1-4)

**Student Model**
- `user`: One-to-one with Django User
- `profile_pic`: Profile image
- `address`: Student address
- `mobile`: Contact number

**Teacher Model**
- `user`: One-to-one with Django User
- `profile_pic`: Profile image
- `address`: Teacher address
- `mobile`: Contact number
- `status`: Approval status
- `salary`: Teacher salary

**Result Model**
- `student`: Foreign key to Student
- `exam`: Foreign key to Course
- `marks`: Marks obtained
- `date`: Exam date/time

---

## Screenshots

### Homepage
![Homepage](static/screenshots/homepage.png)

### Admin Dashboard
![Admin Dashboard](static/screenshots/adminhomepage.png)

### Exam Rules
![Exam Rules](static/screenshots/rules.png)

### Exam Interface
![Exam](static/screenshots/exam.png)

### Teacher Dashboard
![Teacher](static/screenshots/teacher.png)

---

## Known Issues

1. **Question Number Mismatch**: Admin/Teacher can add any number of questions to a course, but the course has a fixed `question_number` field. This can lead to inconsistencies.

2. **CSRF Protection**: CSRF middleware is commented out in settings, which is a security concern for production.

3. **Security Warnings**:
   - `SECRET_KEY` is exposed in settings.py
   - `DEBUG = True` should be False in production
   - `ALLOWED_HOSTS` should be configured for production

4. **Email Configuration**: Requires manual setup and may not work with all email providers.

---

## Security Recommendations

Before deploying to production:

1. **Environment Variables**: Move sensitive data to environment variables
   ```python
   SECRET_KEY = os.environ.get('SECRET_KEY')
   DEBUG = os.environ.get('DEBUG', 'False') == 'True'
   ```

2. **Enable CSRF Protection**: Uncomment CSRF middleware

3. **Configure ALLOWED_HOSTS**:
   ```python
   ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com']
   ```

4. **Use Production Database**: Switch from SQLite to PostgreSQL or MySQL

5. **Static Files**: Configure proper static file serving with WhiteNoise or CDN

---

## Dependencies

```
asgiref==3.2.7
Django==3.0.5
django-widget-tweaks==1.4.8
pytz==2020.1
sqlparse==0.3.1
```

**Note**: Consider updating to newer versions for security patches.

---

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## Future Enhancements

- [ ] Add timer functionality for exams
- [ ] Implement question randomization
- [ ] Add support for different question types (True/False, Fill-in-the-blank)
- [ ] Export results to PDF/Excel
- [ ] Add email notifications for exam results
- [ ] Implement course categories
- [ ] Add student performance analytics
- [ ] Mobile responsive design improvements
- [ ] API endpoints for mobile app integration

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Support

For issues, questions, or suggestions:
- Open an issue on GitHub
- Contact the development team

---

## Acknowledgments

- Built with Django Framework
- UI components enhanced with django-widget-tweaks
- Original developer: Sumit Kumar

---

**Note**: This is a learning/demonstration project. Additional security hardening is required before production deployment.
