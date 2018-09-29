rhvoice-rest
============
[![Docker Pulls](https://img.shields.io/docker/pulls/aculeasis/rhvoice-rest.svg)](https://hub.docker.com/r/aculeasis/rhvoice-rest/)

Это проект на основе синтезатора речи [RHVoice](https://github.com/Olga-Yakovleva/RHVoice)

## Установка
### Быстрый старт

Запуск\обновление из хаба: `./rhvoice_rest.py --upgrade`

Полное описание [тут](https://github.com/Aculeasis/docker-starter)

### Готовый докер
На aarch64 (Например Orange Pi Prime и прочие H5):

- aarch64 `docker run -d -p 8080:8080 aculeasis/rhvoice-rest:arm64v8`
- armv7l `docker run -d -p 8080:8080 aculeasis/rhvoice-rest:arm32v7`
- x86_64 `docker run -d -p 8080:8080 aculeasis/rhvoice-rest:amd64`

Чтобы не терять настройки при обновленях, нужно вынести на хост (через -v): `/opt/cfg`

### Сборка и запуск докера
    git clone https://github.com/Aculeasis/rhvoice-rest
    cd rhvoice-rest
    # Указать Dockerfile под целевую архитектуру
    docker build -t rhvoice-rest -f Dockerfile.arm64v8 .
    docker run -d -p 8080:8080 rhvoice-rest

### Устновка скриптом на debian-based дистрибутивах в качестве сервиса
    git clone https://github.com/Aculeasis/rhvoice-rest
    cd rhvoice-rest
    chmod +x install.sh
    sudo ./install.sh
Статус сервиса `sudo systemctl status rhvoice-rest.service`

#### Многопоточный режим
Для включения запустите с переменной окружения `THREADED=N`, где `N` > 1. Будет запущено `N` процессов синтеза. Потребляет больше ресурсов.
Рекомендуемое значение - не больше чем 1.5 * количество потоков CPU. Если многопоточный доступ не нужен, лучше не включать.

**BUG**: Если сразу дропнуть соединение при первом обращении к процессу он зависнет. Потом (вроде) процессы отвисают и работают нормально. Так делает Chrome, лучше не использовать его для тестирования

## API
    http://SERVER/say?
    text=<текст>
    & voice=<
             aleksandr|anna|elena|irina| # Russian
             alan|bdl|clb|slt| # English
             spomenka| # Esperanto
             natia| # Georgian
             azamat|nazgul| # Kyrgyz
             talgat| # Tatar
             anatol|natalia # Ukrainian
             >
    & format=<wav|mp3|opus>
`SERVER` - Адрес и порт rhvoice-rest. При дефолтной установке на локалхост будет `localhost:8080`.
Конечно, вы можете установить сервер rhvoice-rest на одной машине а клиент на другой. Особенно актуально для слабых одноплатников. 

`text` - URL-encoded строка. Обязательный параметр.

`voice` - голос из RHVoice [полный список](https://github.com/Olga-Yakovleva/RHVoice/wiki/Latest-version-%28Russian%29).
`anna` используется если не задано и в качестве альтернативного спикера.

`format` - Формат возвращаемого файла. Если не задано вернет `mp3`.

## Проверка
<http://localhost:8080/say?text=Привет>

<http://localhost:8080/say?text=Привет%20еще%20раз&format=opus>

<http://localhost:8080/say?text=Kaj%20mi%20ankaŭ%20parolas%20Esperanton&voice=spomenka&format=opus>

## Интеграция
- Home Assistant https://github.com/mgarmash/ha-rhvoice-tts
- [Примеры](https://github.com/Aculeasis/rhvoice-rest/tree/master/example)

## Links
- https://github.com/Olga-Yakovleva/RHVoice
- https://github.com/vantu5z/RHVoice-dictionary
- https://github.com/mgarmash/rhvoice-rest
