# Бюро находок

Бюро находок для города: человек публикует объявление о пропаже или находке с телефона,
сервер сам ищет совпадения и присылает их в «Уведомления». В базе при этом ничего общего
не видно — показывается только результат сравнения.

Проект собран из двух репозиториев, они лежат здесь как подмодули.

---

## Репозитории

- [MobilRing](https://github.com/dragonlex/MobilRing) — Android-приложение: публикация, лента, уведомления, профиль, звонок автору. Kotlin, Jetpack Compose, Firebase.
- [MobilRing-Server2](https://github.com/dragonlex/MobilRing-Server2) — поиск совпадений и подсказка описаний. Python, Firebase RTDB, KoboldCpp.

---

## Как связаны части

1. Приложение пишет объявление в `lost` или `found`, а контакты автора — в `users/{uid}`.
2. `matcher.py` раз в пять минут читает оба узла, оценивает пары и пишет подтверждённые в `matches`.
3. Приложение читает `matches` и показывает карточку только тем, чья заявка в паре участвует.

Подробности по полям и настройкам — в README каждого репозитория.

---

## Быстрый старт

```powershell
git clone --recurse-submodules https://github.com/dragonlex/MobilRing-Project.git
cd MobilRing-Project

cd AndroidApp
$env:JAVA_HOME = "C:\Program Files\Android\Android Studio\jbr"
.\gradlew.bat assembleDebug
adb install -r app\build\outputs\apk\debug\app-debug.apk

cd ..\Server2
.\start-llama.ps1      # модель, отдельное окно
.\start-assist.ps1     # подсказки описаний
.\start-matcher.ps1    # поиск в цикле
```

Если подмодули не подтянулись:

```powershell
git submodule update --init --recursive
```

---

## Состояние

- Приложение: версия 1.3, проверено на Android 10, 14, 16 и на эмуляторе Android 14.
- В приложении есть фото вещи, фильтры ленты, переписка, статус «вещь найдена»,
  настройки приватности и перенос аккаунта по 16-значному ключу.
- Матчер: эвристика и локальная модель, режимы `--once`, `--dry-run`, `--no-llm`.
- Подсказка описаний: `assist.py` на порту 5002.

---

## Что дальше

- Учёт даты потери и находки при сравнении.
- Push-уведомления вместо бейджа в приложении.
- Автопересылка заявок с обозначением «гардероб» на вахту школы.

---

## Авторы

[dragonlex](https://github.com/dragonlex) — приложение и серверная часть
