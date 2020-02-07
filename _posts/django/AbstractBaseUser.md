---
title: 장고 유저 확장
date: 2020-02-07
---

**model.py**  
```django
from django.db import models
from django.contrib.auth.models import (
    BaseUserManager, AbstractBaseUser, PermissionsMixin
)


class MyUserManager(BaseUserManager):
    # 선택적으로 관리자를 migratin으로 직렬화하고 runpython 작업에서 사용 가능하게 할 수 있음
    # 사용자 정의 모델 필드도 migrations에 의해 가져오기 때문에 True 유지해야함
    use_in_migrations = True

    # username, email, password는 필수 그외 나머지 필드들이 들어갈 수 있음
    def _create_user(self, username, email, password, **extra_fields):
        if not username:
            raise ValueError('Users must have an username')
        username = self.model.normalize_username(username)
        user = self.model(
            username=username,
            email=email,
            **extra_fields
        )

        user.set_password(password)
        user.save(using=self._db)
        return user

    def cretate_user(self, username, email, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', False)
        extra_fields.setdefault('is_superuser', False)
        return self._create_user(username, email, password, **extra_fields)

    def create_superuser(self, username, email, password, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)

        if extra_fields.get('is_staff') is not True:
            raise ValueError('Superuser must have is_staff=True.')
        if extra_fields.get('is_superuser') is not True:
            raise ValueError('Superuser must have is_superuser=True.')

        return self._create_user(username, email, password, **extra_fields)


class User(AbstractBaseUser, PermissionsMixin):
    class Meta:
        verbose_name = '회원'
        verbose_name_plural = verbose_name  # 지정하지 않으면 verbose_name에 s를 붙인다

    username = models.CharField(max_length=20, unique=True, verbose_name='유저아이디')
    email = models.EmailField(max_length=255, unique=True, verbose_name='이메일')
    date_of_birth = models.DateField(null=True, blank=True, verbose_name='생일')
    name = models.CharField(
        max_length=15,
        null=True,
        blank=True,
        verbose_name='이름'
    )
    phone_number = models.CharField(
        max_length=15,
        null=True,
        blank=True,
        verbose_name='휴대폰 번호'
    )
    is_active = models.BooleanField(default=True)
    is_staff = models.BooleanField(default=False)

    objects = MyUserManager()

    USERNAME_FIELD = 'username'
    REQUIRED_FIELDS = ['email']

    def __str__(self):
        return self.username

```
**admin.py**
```django
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin as BaseUserAdmin
from .models import User


class UserAdmin(BaseUserAdmin):
    list_display = ('username', 'email', 'is_staff')
    list_filter = ('username', 'email', 'is_staff')
    fieldsets = (
        (None, {'fields': ('username',)}),
        ('개인정보', {'fields': ('email', 'name', 'phone_number', 'date_of_birth')}),
        ('권한', {'fields': ('is_staff',)}),
        ('중요한 일정', {'fields': ('last_login',)}),
    )
    search_fields = ('username',)
    ordering = ('username',)


admin.site.register(User, UserAdmin)

```
