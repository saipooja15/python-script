# python-script
source code
from django.db import models
from django.contrib.auth.models import PermissionsMixin, AbstractBaseUser, BaseUserManager

class User_manager(BaseUserManager):
    def create_user(self, id, real_name, activity period, tz):
        real_name = self.normalize_real_name(real_name)
        user = self.model(id=id, real_name=real_name, activity_period=activity_period, tz=tz)
        

    def create_superuser(self, id, real_name, activity_period, tz):
        user = self.create_user(id=id, real_name=real_name, activity_period=activity_period, tz=tz)
        user.is_superuser = True
        user.is_member = True
        user.save()
        return user



  class User(PermissionsMixin, AbstractBaseUser):
    id = models.CharField(max_length=32, unique=True, )
    real_name = models.EmailField(max_length=32)
    activity_period_choices = [("D", "Date"), ("M", "month"), ("Y", "year")]
    activity_period = models.intField(choices=activity_period_choices, default="none", max_length=1)
    tz = models.CharField(max_length=32, blank=True, null=True)

    is_active = models.BooleanField(default=True)
    is_staff = models.BooleanField(default=False)
    REQUIRED_FIELDS = ["real_name", "activity_period"]
    ID_FIELD = "id"
    objects = User_manager()

    def __str__(self):
        return self.username
