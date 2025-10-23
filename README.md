# AGRICOOL
an e
# create virtual environment
python -m venv env
source env/bin/activate  # (on Windows use: env\Scripts\activate)

# install dependencies
pip install django djangorestframework

# create project
django-admin startproject gava_connect_backend

cd gava_connect_backend

# create app
python manage.py startapp kyc

Django>=5.0
djangorestframework>=3.15

from django.db import models

class User(models.Model):
    national_id = models.CharField(max_length=20, unique=True)
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    date_of_birth = models.DateField()
    gender = models.CharField(max_length=10, null=True, blank=True)
    verified = models.BooleanField(default=False)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"{self.first_name} {self.last_name} ({self.national_id})"


class VerificationLog(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE, related_name="logs")
    status = models.CharField(max_length=20)
    api_response = models.JSONField()
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return f"Log for {self.user.national_id} - {self.status}"
