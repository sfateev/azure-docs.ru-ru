## <a name="microsoft-open-source-code-of-conduct"></a>Правила поведения Майкрософт при работе с проектами с открытым кодом

Этот проект соответствует [правилам поведения Microsoft Open Source](https://opensource.microsoft.com/codeofconduct/).
Дополнительные сведения см. на странице [Вопросы и ответы о правилах поведения](https://opensource.microsoft.com/codeofconduct/faq/) или отправьте вопросы и комментарии по адресу [opencode@microsoft.com](mailto:opencode@microsoft.com).

## <a name="contribute-to-azure-technical-documentation"></a>Редактирование технической документации Azure
Мы приветствуем вклад нашего сообщества (пользователей, клиентов, партнеров, сотрудников MSFT за пределами основных подразделений продуктов Azure и т. д.), а также сотрудников, работающих в основных подразделениях продуктов Azure. Способ участия в редактировании зависит от того, кем вы являетесь.

* **Сообщество — мелкие обновления**. Если вы выполняете мелкие обновления на добровольных началах, вы можете найти статью в нашем репозитории или открыть статью на сайте [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) и щелкнуть в этой статье ссылку **Изменить**, которая ведет на источник статьи на GitHub. Затем можно просто внести изменения в пользовательском интерфейсе GitHub. Или же вы можете создать собственную вилку репозитория и отправлять обновления из нее.

* **Сообщество — новые статьи**. Если вы участвуете в сообществе Azure и намерены создать новую статью, вам потребуется поработать с сотрудником, который поможет внести новое содержимое, выполнив определенные операции в открытом и закрытом репозитории.

* **Сотрудники**. Если вы разработчик технической документации, программный менеджер или разработчик продукта из команды службы Azure и ваша работа состоит в написании или редактировании технических статей, вам следует использовать закрытый репозиторий (https://github.com/MicrosoftDocs/azure-docs-pr). Если вы вносите значительные изменения в существующую статью, добавляете или изменяете изображения либо создаете новую статью, вам необходимо создать вилку полную репозитория, установить Git Bash и редактор Markdown, а также изучить некоторые команды Git. Дополнительные сведения см. в [руководстве внутреннего участника](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master).


## <a name="about-your-contributions-to-azure-content"></a>Внесение изменений в содержимое Azure
### <a name="minor-corrections"></a>Незначительные исправления
Порядок внесения незначительных исправлений или уточнений в документацию и примеры кода из этого репозитория описаны в [условиях использования на сайте docs.microsoft.com](https://docs.microsoft.com/legal/termsofuse).

### <a name="larger-submissions"></a>Значительные изменения
Если запрос на внесение значительных изменений в существующую документацию и примеры кода отправляют не сотрудники корпорации Майкрософт, в комментариях на сайте GitHub появится предложение принять условия лицензионного соглашения участника (CLA). Вам нужно заполнить онлайн-форму, после чего мы сможем принять ваш запрос на внесение изменений.

## <a name="tools-and-setup"></a>Средства и установка
Участники сообщества могут использовать пользовательский интерфейс GitHub или создать вилку репозитория для редактирования. Сотрудникам следует ознакомиться с [руководством внутреннего участника](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) для получения дополнительных сведений о том, как редактировать набор технической документации.

## <a name="repository-organization"></a>Организация репозитория
Содержимое в репозитории azure-docs организовано так же, как и документация на странице https://docs.microsoft.com/azure. Этот репозиторий содержит две корневые папки.

### <a name="articles"></a>\articles
В папке *\articles* содержатся статьи в формате Markdown (файлы с расширением *MD*). Обычно статьи группируются по службе Azure.

Папка *\articles* содержит папку *\media*, предназначенную для файлов мультимедиа из статей корневого каталога. В ней хранятся вложенные папки с изображениями для каждой статьи.  В служебных папках есть отдельная папка media для статей из каждой служебной папки. Папка с изображениями для статьи называется так же, как и файл статьи, но без расширения *MD*.

### <a name="includes"></a>\includes
Вы можете создавать содержимое, которое будет использоваться в нескольких статьях. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Использование разметки Markdown для форматирования статьи
Во всех статьях в этом репозитории используется разметка Markdown, принятая для GitHub.  Вот список полезных ресурсов:

* [Основные сведения о разметке Markdown.](https://help.github.com/articles/markdown-basics/)
* [Подсказки по разметке Markdown в печатном формате.](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)


## <a name="labels"></a>Метки
В открытом репозитории azure-docs к запросам на включение внесенных изменений автоматически добавляются метки. Это позволяет нам управлять рабочими процессами этих запросов, а также предоставлять вам сведения об их состоянии.

* Метки, связанные с лицензионным соглашением участника:
  * cla-not-required — относительно небольшое изменение, не требующее принятия условий CLA;
  * cla-required — достаточно большая область изменений, требующая принятия условий CLA;
  * cla-signed — участник принял условия CLA, в результате чего запрос на включение внесенных изменений можно отправить для проверки.
* Изменения, отправленные автору, — автор получает уведомление о запросе на включение внесенных изменений.
* ready-to-merge — готовность к проверке нашей группой рассмотрения запросов на включение внесенных изменений.


