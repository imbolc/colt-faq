Derby.js
========

Добавить счётчик яндекс-метрики
-------------------------------

views/app/index.html

    <Footer:>
        {{! сюда вставляем код метрики }}


lib/app/index.js

    // Делаем прослойку для всех запросов и сохраняем реферер в модель
    app.get('*', function (page, model, params, next) {
        model.set('_page.referer', params.previous);
        next();
    });

    // Это что-то типа document.ready, работающий корректно и при переходе по ссылкам
    // Запускается только на клиенте. Тут мы сигналим метрике о смене страницы см её API
    // http://help.yandex.ru/metrika/code/ajax-flash.xml
    app.enter('*', function (model) {
        var ref = model.get('_page.referer');
        ref = ref ? location.protocol + '//' + location.hostname +  ref : document.referrer;
        yaCounterXXX.hit(location.href, document.title, ref);
    });
