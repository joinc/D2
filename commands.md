### 0. Запускаем консоль и подгружаем модели
```python
python manage.py shell
from django.contrib.auth.models import User
from NewsPaper.models import Author, Category, Post, PostCategory, Comment
```

### 1. Создать двух пользователей (с помощью метода User.objects.create_user):
```python
User.objects.create_user(username='iivanov',first_name='Иван', last_name='Иванов', email='iivanov@mail.com', password='passw0rd')
User.objects.create_user(username='ppetrov',first_name='Петр', last_name='Петров', email='ppetrov@mail.com', password='passw0rd')
```

### 2. Создать два объекта модели Author, связанные с пользователями:
```python

Author.objects.create(user=User.objects.get(username='iivanov'))
Author.objects.create(user=User.objects.get(username='ppetrov'))
```

### 3. Добавить 4 категории в модель Category:
```python
Category.objects.create(name='Спорт')
Category.objects.create(name='Политика')
Category.objects.create(name='Образование')
Category.objects.create(name='Финансы')
```

### 4. Добавить 2 статьи и 1 новость:
```python
Post.objects.create(author=Author.objects.get(user__username='iivanov'), type_post=0, title='Заголовок статьи Иванова', text='Текст статьи Иванова')
Post.objects.create(author=Author.objects.get(user__username='ppetrov'), type_post=0, title='Заголовок статьи Петрова', text='Текст статьи Петрова')
Post.objects.create(author=Author.objects.get(user__username='iivanov'), type_post=1, title='Заголовок новости Иванова', text='Текст новости Иванова')
```

### 5. Присвоить им категории (как минимум в одной статье/новости должно быть не меньше 2 категорий):
```python
PostCategory.objects.create(post=Post.objects.get(title='Заголовок статьи Иванова'), category=Category.objects.get(name='Спорт'))
PostCategory.objects.create(post=Post.objects.get(title='Заголовок статьи Петрова'), category=Category.objects.get(name='Политика'))
PostCategory.objects.create(post=Post.objects.get(title='Заголовок статьи Петрова'), category=Category.objects.get(name='Финансы'))
PostCategory.objects.create(post=Post.objects.get(title='Заголовок новости Иванова'), category=Category.objects.get(name='Образование'))
```

### 6. Создать как минимум 4 комментария к разным объектам модели Post (в каждом объекте должен быть как минимум один комментарий):
```python
Comment.objects.create(post=Post.objects.get(title='Заголовок статьи Иванова'), user=User.objects.get(username='ppetrov'), text='Первый комментарий Петрова к статье Иванова')
Comment.objects.create(post=Post.objects.get(title='Заголовок статьи Иванова'), user=User.objects.get(username='iivanov'), text='Второй комментарий Иванова к статье Иванова')
Comment.objects.create(post=Post.objects.get(title='Заголовок статьи Петрова'), user=User.objects.get(username='iivanov'), text='Первый комментарий Иванова к статье Петрова')
Comment.objects.create(post=Post.objects.get(title='Заголовок новости Иванова'), user=User.objects.get(username='ppetrov'), text='Первый комментарий Петрова к новости Иванова')
```

### 7. Применяя функции like() и dislike() к статьям/новостям и комментариям, скорректировать рейтинги этих объектов:
```python
Post.objects.get(title='Заголовок статьи Иванова').like()
Post.objects.get(title='Заголовок статьи Иванова').like()
Post.objects.get(title='Заголовок статьи Иванова').dislike()
Post.objects.get(title='Заголовок статьи Иванова').like()
Post.objects.get(title='Заголовок статьи Петрова').like()
Post.objects.get(title='Заголовок статьи Петрова').like()
Post.objects.get(title='Заголовок статьи Петрова').like()
Post.objects.get(title='Заголовок статьи Петрова').like()
Post.objects.get(title='Заголовок новости Иванова').dislike()
Post.objects.get(title='Заголовок новости Иванова').like()
Post.objects.get(title='Заголовок новости Иванова').like()

Comment.objects.get(text='Первый комментарий Петрова к статье Иванова').like()
Comment.objects.get(text='Первый комментарий Петрова к статье Иванова').like()
Comment.objects.get(text='Первый комментарий Петрова к статье Иванова').like()
Comment.objects.get(text='Первый комментарий Петрова к статье Иванова').like()
Comment.objects.get(text='Второй комментарий Иванова к статье Иванова').like()
Comment.objects.get(text='Второй комментарий Иванова к статье Иванова').dislike()
Comment.objects.get(text='Второй комментарий Иванова к статье Иванова').like()
Comment.objects.get(text='Первый комментарий Иванова к статье Петрова').like()
Comment.objects.get(text='Первый комментарий Иванова к статье Петрова').like()
Comment.objects.get(text='Первый комментарий Иванова к статье Петрова').dislike()
Comment.objects.get(text='Первый комментарий Иванова к статье Петрова').like()
Comment.objects.get(text='Первый комментарий Петрова к новости Иванова').like()
Comment.objects.get(text='Первый комментарий Петрова к новости Иванова').like()
Comment.objects.get(text='Первый комментарий Петрова к новости Иванова').like()
Comment.objects.get(text='Первый комментарий Петрова к новости Иванова').like()
```

### 8. Обновить рейтинги пользователей:
```python
Author.objects.get(user__username='iivanov').update_rating()
Author.objects.get(user__username='ppetrov').update_rating()
```

### 9. Вывести username и рейтинг лучшего пользователя (применяя сортировку и возвращая поля первого объекта):
```python
best_author = Author.objects.order_by('-rating').first()
print(f'{best_author.user.username} - {best_author.rating}')
```

### 10. Вывести дату добавления, username автора, рейтинг, заголовок и превью лучшей статьи, основываясь на лайках/дислайках к этой статье:
```python
best_post = Post.objects.order_by('-rating').first()       
print(f'{best_post.create_date.strftime("%d %B %Y")} {best_post.author.user.username} {best_post.rating} {best_post.title} {best_post.preview()}')
```

### 11. Вывести все комментарии (дата, пользователь, рейтинг, текст) к этой статье:
```python
print(*[f'{comment.create_date.strftime("%d %B %Y")} {comment.user.username} {comment.rating} {comment.text}' for comment in Comment.objects.filter(post=Post.objects.order_by('-rating').first())], sep='\n')
```
