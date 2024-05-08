# The-Learning-Agency-Lab---PII-Data-Detection
# GitHub Overview

[GitHub](https://github.com/) is a web-based hosting service for version control and collaboration. It allows multiple people to work on projects simultaneously, making it easy for them to collaborate and contribute. GitHub is primarily used by programmers for version control of their code.

In the context of this competition, GitHub could be used for sharing and collaborating on the development of the PII detection model. Participants can work on different parts of the project, track changes, discuss improvements, and submit modifications for consideration.

GitHub supports several programming languages and offers a robust set of features such as bug tracking, task management, and wikis for every project. GitHub also provides a platform for users to publish their datasets and share their models with a wide audience.

With its user-friendly interface and diverse community of developers, GitHub is an ideal platform for open-source projects and collaborative endeavors in the field of data science and beyond.




# Решение: Использование 14 моделей в ансамбле и постобработка 🚀

Прежде всего, хочу выразить благодарность всем, кто обучал выдающиеся модели, использованные в этом ноутбуке. Особая благодарность тем, кто создавал и публиковал свои ноутбуки, делится комментариями и находками.

## Использование моделей во время инференса 📊

В процессе инференса мы использовали 14 моделей из следующих публичных датасетов:

- [PII DeBERTa Models от Verracodeguacas](https://www.kaggle.com/datasets/verracodeguacas/pii-deberta-models)
- [37vp4pjt от Emiz6413](https://www.kaggle.com/datasets/emiz6413/37vp4pjt)
- [PII Models от Startalks](https://www.kaggle.com/datasets/startalks/pii-models)

Отдельная благодарность @emiz6413 за его выдающийся ноутбук и модель с описанием процесса обучения.

### Ансамбль моделей 🌐

В ансамбле использовалось большое количество моделей: в первые три часа лучшие модели делали предсказания на всём наборе данных, а в оставшиеся пять с половиной часов оставшиеся модели делали дополнительные предсказания на 2/3 данных с короткой длиной токенов.

Мы предположили, что количество PII-токенов не так сильно зависит от длины текста, как время предсказания, которое зависит от длины текста. Поэтому было выгоднее использовать большее количество моделей на текстах с короткими токенами и меньшее количество моделей на текстах с длинными токенами.

Кроме того, для разных типов меток устанавливались разные пороги вероятности. Для имен учеников порог был ниже. Для других меток порог был выше, так как таких меток в обучающем наборе данных было меньше, и мы предположили, что модель хуже училась их находить, и при низком пороге предсказывала бы много лишних меток, чем для имен учеников.

### Постобработка 🛠️

Имена учеников должны начинаться с заглавной буквы и продолжаться только строчными буквами:
`r'^[A-Z][a-z]+$'`
Для токенов "B-", за которыми не следует "I-", удалялись слишком короткие токены, а также номера телефонов и адреса электронной почты, и токены B-ID_NUM, которые не соответствовали шаблону. Например, для B-ID_NUM должно было быть хотя бы две последовательные цифры в токене, и он должен был быть длиной не менее 4 символов. Токены "B-", за которыми следует "I-", не очищались этими дополнительными шаблонами.
Метки PII добавлялись для токенов с адресами, если в адресе не было отмечены разрывы строк и другие токены.

## Заключение 👏

Благодарю своего коллегу @wasjaip. Это было интересное время и хорошее обучение.
