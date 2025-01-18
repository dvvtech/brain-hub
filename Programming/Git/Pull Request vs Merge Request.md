Pull request GitHub
Merge request в GitLab

Новая ветка
```bash
git checkout -b my-new-feature
```

Внесение изменений
```bash
git add .
git commit -m "Добавил новую фичу"
```

Пушим ветку в удаленный репозиторий
```bash
git push origin my-new-feature
```

Создание MergeRequest в UI (будет кнопка merge request)
и нужно будет заполнить поля и выбрать какую ветку слить в какую слить

Рецензирование кода

Внесение исправлений
```bash
git commit -am "Внес исправления"
git push origin my-new-feature
```

Слияние веток