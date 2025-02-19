# Вопросы по веб-разработке с собеседований

1. Что происходит после того, как вы вводите название сайта в браузер и нажимаете Enter? Подробно объяснить.
    <details>
      <summary>Ответ</summary>

      #### *1. Парсинг URL - можно отнести к условно первому этапу (не считая самого процесса ввода символов в поисковом поле браузера), во время которого*:
      - Браузер проверяет список предзагруженных HSTS (HTTP Strict Transport Security). Это список сайтов, которые требуют, чтобы к ним обращались только по HTTPS. Если нужный сайт есть в этом списке, то браузер отправляет ему запрос через HTTPS вместо HTTP. В противном случае, начальный запрос посылается по HTTP.
      - Конвертируются не-ASCII Unicode символы в название хоста.

      #### *2. Следующий этап - Определение DNS*
      - Браузер проверяет наличие домена в своём кэше. Если домена там нет, то браузер вызывает библиотечную функцию *gethostbyname* (отличается в разных ОС) для поиска нужного адреса в файле *hosts*.
      - Если домен нигде не закэширован и отсутствует в файле hosts, gethostbyname отправляет запрос к сетевому DNS-серверу.
      - Запрос к сетевому DNS-серверу называется ARP-запросом.
      - Для того, чтобы отправить ARP-запрос браузеру необходимо отыскать целевой IP-адрес, а также знать MAC-адрес интерфейса, который будет использоваться для отправки ARP-запроса.

      #### *3. Открытие сокета и сборка TCP-сегмента/пакета*
      - Когда браузер получает IP-адрес конечного сервера, то он берёт эту информацию и данные об используемом порте из URL (80 порт для HTTP, 443 для HTTPS), осуществляет вызов функции socket системной библиотеки и запрашивает поток TCP сокета.
      - Этот запрос сначала проходит через транспортный уровень, где собирается TCP-сегмент. Получившийся сегмент отправляется на сетевой уровень, на котором добавляется дополнительный IP-заголовок, IP-адрес сервера назначения и адрес текущей машины — теперь сегмент сформирован.

      #### *4. TLS handshake — для передачи пакетов данных между клиентом (компьютером) и сервером важно установить TCP-соединение. Это соединение устанавливается с помощью процесса, называемого трехсторонним рукопожатием TCP / IP, реализованного следующим образом*:

      - Клиентская машина отправляет SYN-пакет на сервер, спрашивая, открыт ли он для новых подключений.
      - Если на сервере есть открытые порты, которые могут принимать и инициировать новые соединения, он ответит, используя пакет SYN / ACK.
      - Клиент получит пакет SYN / ACK от сервера и подтвердит его, отправив пакет ACK.

      #### *5. Обработка HTTP-запросов на сервере*
      - Одним из инструментов обработки запросов/ответов на стороне сервера является  HTTPD. Наиболее популярные HTTPD-серверы это Apache или Nginx для Linux и IIS для Windows.
      - Сервер разбирает запрос по следующим параметрам: метод HTTP-запроса (наиболее распространенные — GET/POST), домен, запрашиваемые пути.
      - Сервер находит контент, который соответствует запросу и парсит файл с помощью обработчика.

      #### *6. Парсинг HTML и интерпретация CSS*
      - Главной задачей HTML-парсера является разбор разметки в специальное дерево — «parse tree» — это дерево DOM-элементов.
      - Во время разбора браузер парсит CSS-файлы, каждый из которых разбирается в объект StyleSheet.

      #### *7. Рендеринг страниц и пост-рендеринговое исполнение*
      - Путём перебора DOM-узлов и вычисления для каждого узла значений CSS-стилей создаётся «Дерево рендера» (Render Tree или Frame Tree).
      - Вычисляются координаты каждого узла. Вычисляются финальные позиции слоёв и через Direct3D/OpenGL отдаются композитные команды.
      - После завершения рендеринга, браузер исполняет JavaScript-код в результате срабатывания  часового механизма или в результате действий пользователя.

      #### *Для более детального ознакомления:*
      - https://habr.com/ru/company/htmlacademy/blog/254825/
      - https://github.com/alex/what-happens-when
      - https://medium.com/@maneesha.wijesinghe1/what-happens-when-you-type-an-url-in-the-browser-and-press-enter-bb0aa2449c1a
      - https://medium.com/launch-school/demystifying-ruby-applications-ruby-application-servers-and-web-servers-c3d0fd415cb3
    </details>

1. Что такое сессия? Для чего используется?
1. Что такое процессы? Как с ними работать?
1. Что такое DNS?
1. Настройка http-серверов.
1. Сравните сервер аппликейшены и сервис апликейшены (Nginx, Apache, Puma, Webrick).
1. Nginx, его отличие от apache?

    <details>
      <summary>Ответ</summary>
      Два самых широко распространенных веб-сервера. Если рассматривать жизненные примеры, то основные различия между Apache и Nginx в том как они обрабатывают запросы к статическому и динамическому контенту.
    </details>

1. Почему используют не Puma, а Nginx?
1. Балансировка нагрузки на сервера приложений (haproxy).
1. Чем отличается HTTP от HTTPS?

    <details>
      <summary>Ответ</summary>

      * HyperText Transfer Protocol

      * HyperText Transfer Protocol Secure — расширение протокола HTTP для поддержки шифрования в целях повышения безопасности.
    </details>

1. Из чего состоит HTTP запрос?
1. Что такое HTTP OPTIONS?

    <details>
      <summary>Ответ</summary>

     Используется для определения возможностей веб-сервера или параметров соединения для конкретного ресурса.
     В ответ серверу следует включить заголовок Allow со списком поддерживаемых методов.
     Также в заголовке ответа может включаться информация о поддерживаемых расширениях.
    </details>

    https://developer.mozilla.org/ru/docs/Web/HTTP/Methods/OPTIONS
    https://ru.wikipedia.org/wiki/HTTP#OPTIONS
    https://habr.com/ru/post/342432/
1. Какое шифрование имеет HTTPS?

    <details>
      <summary>Ответ</summary>

     Защиту данных в HTTPS обеспечивает криптографический протокол SSL/TLS,
     который шифрует передаваемую информацию. По сути этот протокол является обёрткой для HTTP.
     Он обеспечивает шифрование данных и делает их недоступными для просмотра посторонними.
     Протокол SSL/TLS хорош тем, что позволяет двум незнакомым
     между собой участникам сети установить защищённое соединение через незащищённый канал.

     https://yandex.ru/blog/company/77455
     https://habr.com/ru/post/188042/
     https://firstssl.ru/faq/general-questions/chto-takoe-https
    </details>

1. Что такое сессии? Для чего нужны?
1. Какое максимальное количествол кб, может весить 1 кука в сессии RoR?

    <details>
      <summary>Ответ</summary>
      4 kb
    </details>

1. Что такое crontab и для чего используются?
1. Как устроен HTTP, какие есть заголовки, как работают cookies, как работает HTTPS?

1. Реализация протокола AMQP

    <details>
      <summary>Ответ</summary>
      AMQP протокол используется для передачи данных между компонентами системы. Основная идея состоит в том, что отдельные подсистемы (или независимые приложения) могут обмениваться произвольным образом сообщениями через AMQP-брокер, который осуществляет маршрутизацию, возможно гарантирует доставку, распределение потоков данных, подписку на нужные типы сообщений.

      Брокеры: RabbitMQ (гем `bunny`), ActiveMQ, Apache Kafka.

      https://ru.wikipedia.org/wiki/AMQP
      https://kt.team/hr/blog/rabbitmq
      https://www.bigdataschool.ru/blog/kafka-vs-rabbitmq-big-data.html#:~:text=%D0%A7%D0%B5%D0%BC%20%D0%9A%D0%B0%D1%84%D0%BA%D0%B0%20%D0%BE%D1%82%D0%BB%D0%B8%D1%87%D0%B0%D0%B5%D1%82%D1%81%D1%8F%20%D0%BE%D1%82%20%D0%9A%D1%80%D0%BE%D0%BB%D0%B8%D0%BA%D0%B0,(topic)%20%D0%BD%D1%83%D0%B6%D0%BD%D1%8B%D0%B5%20%D0%B8%D0%BC%20%D1%81%D0%BE%D0%BE%D0%B1%D1%89%D0%B5%D0%BD%D0%B8%D1%8F.
      https://habr.com/ru/company/itsumma/blog/416629/
    </details>

1. Что такое OSI-модель? Сколько уровней она имеет, опишите их.

    <details>
      <summary>Ответ</summary>
      Модель Open Systems Interconnection (OSI) – это скелет, фундамент и база всех сетевых сущностей. Модель определяет сетевые протоколы, распределяя их на 7 логических уровней. Важно отметить, что в любом процессе, управление сетевой передачей переходит от уровня к уровню, последовательно подключая протоколы на каждом из уровней.

      https://ru.wikipedia.org/wiki/%D0%A1%D0%B5%D1%82%D0%B5%D0%B2%D0%B0%D1%8F_%D0%BC%D0%BE%D0%B4%D0%B5%D0%BB%D1%8C_OSI
      https://wiki.merionet.ru/seti/18/model-osi-eto-prosto/
    </details>

1. Назовите отличия протокола TCP и UDP.

    <details>
      <summary>Ответ</summary>
      Протокол TCP (Transmission Control Protocol) – это сетевой протокол, который «заточен» под соединение. Иными словами, прежде, чем начать обмен данными, данному протоколу требуется установить соединение между двумя хостами. Данный протокол имеет высокую надежность, поскольку позволяет не терять данные при передаче, запрашивает подтверждения о получении от принимающей стороны и в случае необходимости отправляет данные повторно. При этом отправляемые пакеты данных сохраняют порядок отправки, то есть можно сказать, что передача данных упорядочена. Минусом данного протокола является относительно низкая скорость передачи данных, за счет того что выполнение надежной и упорядоченной передачи занимает больше времени, чем в альтернативном протоколе UDP.

      Протокол UDP (User Datagram Protocol), в свою очередь, более прост. Для передачи данных ему не обязательно устанавливать соединение между отправителем и получателем. Информация передается без предварительной проверки готовности принимающей стороны. Это делает протокол менее надежным – при передаче некоторые фрагменты данных могут теряться. Кроме того, упорядоченность данных не соблюдается – возможен непоследовательный прием данных получателем. Зато скорость передачи данных по данному транспортному протоколу будет более высокой.

      https://wiki.merionet.ru/seti/23/tcp-i-udp-v-chem-raznica/
      http://pyatilistnik.org/chem-otlichaetsya-protokol-tcp-ot-udp/
      https://yandex.ru/q/question/computers/chem_otlichaetsia_tcp_ot_udp_66a93f75/
      https://habr.com/ru/company/oleg-bunin/blog/461829/
    </details>

1. Что такое RPC?

    <details>
      <summary>Ответ</summary>
        RPC (от англ. Remote Procedure Call, RPC) Удалённый вызов процедур, реже Вызов удалённых процедур — класс технологий, позволяющих компьютерным программам вызывать функции или процедуры в другом адресном пространстве (на удалённых компьютерах, либо в независимой сторонней системе на том же устройстве). Обычно реализация RPC-технологии включает в себя два компонента: сетевой протокол для обмена в режиме клиент-сервер и язык сериализации объектов (или структур, для необъектных RPC).

      https://ru.wikipedia.org/wiki/%D0%A3%D0%B4%D0%B0%D0%BB%D1%91%D0%BD%D0%BD%D1%8B%D0%B9_%D0%B2%D1%8B%D0%B7%D0%BE%D0%B2_%D0%BF%D1%80%D0%BE%D1%86%D0%B5%D0%B4%D1%83%D1%80
    </details>
