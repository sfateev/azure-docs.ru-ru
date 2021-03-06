---
title: Мониторинг выходных данных runbook в службе автоматизации Azure
description: В этой статье рассказывается, как осуществлять мониторинг выходных данных и сообщений последовательностей runbook.
services: automation
ms.subservice: process-automation
ms.date: 12/04/2018
ms.topic: conceptual
ms.openlocfilehash: e4be7934002730253b77b1c129165ad9f19f23b7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86185982"
---
# <a name="monitor-runbook-output"></a>Мониторинг выходных данных runbook

В большинстве модулей runbook в службе автоматизации Azure используются выходные данные определенного типа. Это сообщение об ошибке для пользователя или составной объект, предназначенный для использования с другой последовательностью runbook. Windows PowerShell предоставляет [несколько потоков](/powershell/module/microsoft.powershell.core/about/about_redirection) для отправки выходных данных из сценария или рабочего процесса. Служба автоматизации Azure работает с каждым из этих потоков по-разному. При создании runbook необходимо следовать рекомендациям по использованию потоков.

В следующей таблице приводится краткое описание каждого потока с указанием его поведения на портале Azure при работе с опубликованными последовательностями runbook и при их [тестировании](./manage-runbooks.md). Поток вывода — это основной поток, используемый для обмена данными между последовательностями runbook. Остальные потоки классифицируются как потоки сообщений, предназначенные для передачи информации пользователю. 

| STREAM | Описание | Опубликован | Тест |
|:--- |:--- |:--- |:--- |
| Error |Сообщение об ошибке, предназначенное для пользователя. В отличие от исключения, по умолчанию runbook продолжит работу после сообщения об ошибке. |Записывается в журнал заданий |Отображается в области вывода теста |
| Отладка |Сообщения, предназначенные для интерактивного пользователя. Не следует использовать в модулях runbook. |Не записывается в журнал заданий |Не отображается в области вывода теста |
| Выходные данные |Объекты, предназначенные для использования другими Runbook. |Записывается в журнал заданий |Отображается в области вывода теста |
| Ход выполнения |До и после каждого действия в Runbook автоматически создаются записи. Нет необходимости, чтобы последовательность runbook создавала собственные записи хода выполнения, так как они предназначены для интерактивного пользователя. |Записывается в журнал заданий только в том случае, если для runbook включено ведение журнала хода выполнения |Не отображается в области вывода теста |
| Подробный |Сообщения, содержащие общую или отладочную информацию. |Записывается в журнал заданий только в том случае, если для runbook включено подробное ведение журнала |Отображается в области вывода теста только в том случае, если в runbook для переменной `VerbosePreference` задано значение Continue |
| Предупреждение |Предупреждающее сообщение, предназначенное для пользователя. |Записывается в журнал заданий |Отображается в области вывода теста |

## <a name="use-the-output-stream"></a>Использование потока вывода

Поток вывода предназначен для вывода объектов, созданных скриптом или рабочим процессом, когда он работает правильно. В службе автоматизации Azure этот поток в основном используется для объектов, которые предназначены для родительской последовательности runbook, вызывающей [текущую](automation-child-runbooks.md). Когда родительская последовательность runbook [вызывает встроенную](automation-child-runbooks.md#invoke-a-child-runbook-using-inline-execution), дочерняя возвращает родительской данные из потока вывода. 

Последовательность runbook использует поток вывода для передачи общей информации клиенту, только если она никогда не вызывалась другой runbook. Однако в обычном случае для передачи информации пользователю рекомендуется, чтобы последовательность runbook использовала [подробный поток](#monitor-verbose-stream).

Последовательность runbook должна записать данные в поток вывода с помощью [Write-Output](/powershell/module/microsoft.powershell.utility/write-output). Кроме того, можно поместить объект в отдельную строку скрипта.

```powershell
#The following lines both write an object to the output stream.
Write-Output –InputObject $object
$object
```

### <a name="handle-output-from-a-function"></a>Обработка выходных данных функции

Когда функция runbook выполняет запись в поток вывода, выходные данные возвращаются runbook. Если runbook присваивает эти выходные данные переменной, то они не записываются в поток вывода. Запись в любые другие потоки из функции сопровождается записью в соответствующий поток для runbook. Рассмотрим следующий пример runbook рабочего процесса PowerShell.

```powershell
Workflow Test-Runbook
{
  Write-Verbose "Verbose outside of function" -Verbose
  Write-Output "Output outside of function"
  $functionOutput = Test-Function
  $functionOutput

  Function Test-Function
  {
    Write-Verbose "Verbose inside of function" -Verbose
    Write-Output "Output inside of function"
  }
}
```

Поток вывода для задания runbook будет следующим:

```output
Output inside of function
Output outside of function
```

Подробный поток для задания runbook будет следующим:

```output
Verbose outside of function
Verbose inside of function
```

После публикации последовательности runbook и перед ее запуском следует включить подробное ведение журнала в параметрах runbook, чтобы получить выходные данные подробного потока.

### <a name="declare-output-data-type"></a>Объявление типа выходных данных

Ниже приведены примеры типов выходных данных.

* `System.String`
* `System.Int32`
* `System.Collections.Hashtable`
* `Microsoft.Azure.Commands.Compute.Models.PSVirtualMachine`

#### <a name="declare-output-data-type-in-a-workflow"></a>Объявление типа выходных данных в рабочем процессе

Рабочий процесс указывает тип выходных данных с помощью [атрибута OutputType](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute). Этот атрибут не оказывает влияния во время выполнения, но позволяет получить указания об ожидаемых выходных данных runbook во время разработки. Так как набор инструментов для последовательностей runbook продолжает увеличиваться, растет и важность объявления типов выходных данных во время разработки. Поэтому рекомендуется включать это объявление во все создаваемые последовательности runbook.

Следующий пример Runbook выводит строковый объект и включает в себя объявление типа выходных данных. Если Runbook выводит массив определенного типа, все равно следует указать его тип, а не массив типа.

```powershell
Workflow Test-Runbook
{
  [OutputType([string])]

  $output = "This is some string output."
  Write-Output $output
}
 ```

#### <a name="declare-output-data-type-in-a-graphical-runbook"></a>Объявление типа выходных данных в графической последовательности runbook

Чтобы объявить тип выходных данных в графических последовательностях runbook или графических последовательностях runbook рабочего процесса PowerShell, в разделе **Входные и выходные данные** укажите тип выходных данных. Рекомендуем использовать полное имя класса .NET, чтобы упростить идентификацию при ссылке на них из родительской последовательности runbook. При использовании полного имени все свойства этого класса предоставляются шине данных в runbook. Это повышает гибкость при использовании свойств для условной логики, ведения журнала и ссылки на них как на значение для других действий в последовательности runbook.<br> ![Runbook: раздел "Входные и выходные данные"](media/automation-runbook-output-and-messages/runbook-menu-input-and-output-option.png)

>[!NOTE]
>После ввода значения в поле **Тип выходных данных** щелкните за пределами элемента управления в области свойств входных и выходных данных, чтобы он распознал вашу запись.

Ниже приведен пример с использованием двух графических последовательностей runbook, демонстрирующий работу компонента "Входные и выходные данные". Если применить модульную модель проектирования последовательностей runbook, одна такая последовательность используется в качестве шаблона проверки подлинности runbook для управления аутентификацией в Azure с помощью учетной записи запуска от имени. Вторая последовательность runbook, которая обычно выполняет базовую логику для автоматизации данного сценария, в этом случае выполняет шаблон проверки подлинности runbook. Результаты отображаются в области выходных данных теста. В обычных условиях этот модуль runbook выполнял бы определенные действия в отношении ресурса, используя выходные данные дочернего модуля runbook.

Ниже представлена основная логика модуля Runbook **AuthenticateTo-Azure**.<br> ![Runbook: пример шаблона аутентификации](media/automation-runbook-output-and-messages/runbook-authentication-template.png).

Последовательность runbook включает тип выходных данных `Microsoft.Azure.Commands.Profile.Models.PSAzureContext`, который возвращает свойства профиля проверки подлинности.<br> ![Runbook: пример типа выходных данных](media/automation-runbook-output-and-messages/runbook-input-and-output-add-blade.png)

Хотя эта последовательность runbook достаточно проста, в ней есть элемент конфигурации, о котором стоит упомянуть. Последнее действие выполняет командлет `Write-Output` для записи данных профиля в переменную с помощью выражения PowerShell для параметра `Inputobject`. Этот параметр является обязательным для `Write-Output`.

Вторая последовательность runbook с именем **Test-ChildOutputType** в нашем примере просто определяет два действия.<br> ![Runbook: пример типа выходных данных дочернего модуля](media/automation-runbook-output-and-messages/runbook-display-authentication-results-example.png)

Первое действие вызывает runbook с именем **AuthenticateTo-Azure**. Второе действие запускает командлет `Write-Verbose`, у которого в качестве **источника данных** указаны **выходные данные действия**. Кроме того, параметр **Путь к полю** имеет значение **Context.Subscription.SubscriptionName**, которое представляет выходные данные контекста из runbook с именем **AuthenticateTo-Azure**.<br> ![Источник данных параметров в командлете Write-Verbose](media/automation-runbook-output-and-messages/runbook-write-verbose-parameters-config.png)

Результат — имя подписки.<br> ![Runbook: результаты командлета Test-ChildOutputType](media/automation-runbook-output-and-messages/runbook-test-childoutputtype-results.png)

## <a name="monitor-message-streams"></a>Мониторинг потоков сообщений

В отличие от потока вывода потоки сообщений передают информацию пользователю. Существует несколько потоков сообщений для различных типов информации, и служба автоматизации Azure обрабатывает каждый из них по-разному.

### <a name="monitor-warning-and-error-streams"></a>Мониторинг потоков предупреждений и ошибок

Потоки предупреждений и ошибок используются для ведения журнала проблем, возникающих в runbook. Служба автоматизации Azure записывает эти потоки в журнал заданий при выполнении runbook. В службе автоматизации потоки отображаются на портале Azure при тестировании runbook в области выходных данных теста. 

По умолчанию выполнение runbook продолжается после предупреждения или ошибки. Можно указать, что runbook следует приостанавливать при предупреждении или ошибке. Для этого в runbook перед созданием сообщения должна быть задана [привилегированная переменная](#work-with-preference-variables). Например, чтобы приостанавливать runbook при ошибке, как при исключении, присвойте переменной `ErrorActionPreference` значение Stop.

Создайте предупреждение или сообщение об ошибке с помощью командлета [Write-Warning](/powershell/module/microsoft.powershell.utility/write-warning) или [Write-Error](/powershell/module/microsoft.powershell.utility/write-error). Действия также могут записывать в потоки предупреждений и ошибок.

```powershell
#The following lines create a warning message and then an error message that will suspend the runbook.

$ErrorActionPreference = "Stop"
Write-Warning –Message "This is a warning message."
Write-Error –Message "This is an error message that will stop the runbook because of the preference variable."
```

### <a name="monitor-debug-stream"></a>Мониторинг потока отладки

Служба автоматизации Azure использует поток сообщений отладки для интерактивных пользователей. Его не следует использовать в runbook.

### <a name="monitor-verbose-stream"></a>Мониторинг подробного потока

Поток подробных сообщений поддерживает передачу общих сведений о работе runbook. Так как в последовательности runbook недоступен поток отладки, в качестве источника информации для отладки в ней должны использоваться подробные сообщения. 

По умолчанию подробные сообщения от опубликованных runbook не хранятся в журнале заданий по соображениям производительности. Для хранения подробных сообщений на портале Azure перейдите на вкладку **Настройка** и выберите параметр **Вести подробный журнал**, чтобы опубликованные последовательности runbook записывали в журнал подробные сообщения. Включайте этот параметр только для устранения неполадок и отладки Runbook. В большинстве случаев для runbook следует оставить значение по умолчанию, при котором подробные записи в журнал не добавляются.

При [тестировании модуля runbook](./manage-runbooks.md) подробные сообщения не отображаются, даже если для runbook настроено добавление подробных записей в журнал. Для отображения подробных сообщений [при тестировании runbook](./manage-runbooks.md) необходимо задать для переменной `VerbosePreference` значение Continue. После этого подробные сообщения будут отображаться в области вывода теста на портале Azure.

Приведенный ниже код создает подробное сообщение, используя командлет [Write-Verbose](/powershell/module/microsoft.powershell.utility/write-verbose).

```powershell
#The following line creates a verbose message.

Write-Verbose –Message "This is a verbose message."
```

## <a name="handle-progress-records"></a>Обработка записей ведения журнала

Чтобы настроить runbook для записи в журнал данных о ходе выполнения, на портале Azure перейдите на вкладку **Настройка**. Чтобы обеспечить максимальную производительность, такие записи в журнал по умолчанию не добавляются. В большинстве случаев следует использовать параметр по умолчанию. Включайте этот параметр только для устранения неполадок и отладки Runbook. 

Если включить ведение журнала записей о ходе выполнения, runbook будет добавлять в журнал заданий запись до и после выполнения каждого действия. При тестировании последовательности runbook сообщения о ходе выполнения не отображаются, даже если для нее настроено добавление записей о ходе выполнения в журнал.

>[!NOTE]
>Командлет [Write-Progress](/powershell/module/microsoft.powershell.utility/write-progress) недействителен в runbook, так как предназначен для использования интерактивным пользователем.

## <a name="work-with-preference-variables"></a>Использование привилегированных переменных

В последовательностях runbook можно задать определенные [привилегированные переменные](/powershell/module/microsoft.powershell.core/about/about_preference_variables) Windows PowerShell для управления ответом на данные, отправляемые в различные потоки вывода. В следующей таблице перечислены привилегированные переменные, которые могут использоваться в последовательностях runbook, а также их значения по умолчанию и допустимые значения. При использовании в Windows PowerShell за пределами службы автоматизации Azure поддерживаются дополнительные значения привилегированных переменных.

| Переменная | Значение по умолчанию | Допустимые значения |
|:--- |:--- |:--- |
| `WarningPreference` |Continue |Остановить<br>Continue<br>SilentlyContinue |
| `ErrorActionPreference` |Continue |Остановить<br>Continue<br>SilentlyContinue |
| `VerbosePreference` |SilentlyContinue |Остановить<br>Continue<br>SilentlyContinue |

В следующей таблице представлено поведение для соответствующих значений привилегированных переменных, допустимых в runbook.

| Значение | Поведение |
|:--- |:--- |
| Continue |Записывает сообщение в журнал и продолжает выполнение Runbook. |
| SilentlyContinue |Продолжает выполнение Runbook без добавления сообщения в журнал. При таком значении сообщение игнорируется. |
| Остановить |Записывает сообщение в журнал и приостанавливает выполнение Runbook. |

## <a name="retrieve-runbook-output-and-messages"></a><a name="runbook-output"></a>Извлечение выходных данных и сообщений runbook

### <a name="retrieve-runbook-output-and-messages-in-azure-portal"></a>Извлечение выходных данных и сообщений runbook на портале Azure

На портале Azure на вкладке **Задания** для runbook можно просмотреть подробную информацию о задании runbook. Помимо общей информации о задании, в сводке будут перечислены входные параметры, [поток вывода](#use-the-output-stream), а также все возникшие исключения. Журнал заданий содержит сообщения из потоков вывода, [предупреждений и ошибок](#monitor-warning-and-error-streams). Он также содержит сообщения из [подробного потока](#monitor-verbose-stream) и [записи о ходе выполнения](#handle-progress-records), если для runbook настроено добавление этих данных в журнал.

### <a name="retrieve-runbook-output-and-messages-in-windows-powershell"></a>Извлечение выходных данных и сообщений runbook в Windows PowerShell

В Windows PowerShell выходные данные и сообщения из runbook можно извлечь с помощью командлета [Get-AzAutomationJobOutput](/powershell/module/Az.Automation/Get-AzAutomationJobOutput?view=azps-3.5.0). Для этого командлета требуется идентификатор задания. У него также есть параметр `Stream`, с помощью которого можно указать извлекаемый поток. Указав значение Any для этого параметра, можно получить все потоки для задания.

В следующем примере запускается пример Runbook и ожидается его завершение. После завершения выполнения runbook скрипт собирает данные потока вывода runbook из задания.

```powershell
$job = Start-AzAutomationRunbook -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook"

$doLoop = $true
While ($doLoop) {
  $job = Get-AzAutomationJob -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" -Id $job.JobId
  $status = $job.Status
  $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Output

# For more detailed job output, pipe the output of Get-AzAutomationJobOutput to Get-AzAutomationJobOutputRecord
Get-AzAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
  –AutomationAccountName "MyAutomationAccount" -Id $job.JobId –Stream Any | Get-AzAutomationJobOutputRecord
```

### <a name="retrieve-runbook-output-and-messages-in-graphical-runbooks"></a>Извлечение выходных данных и сообщений runbook в графических последовательностях runbook

Для графических последовательностей runbook дополнительное ведение журнала выходных данных и сообщений доступно в виде трассировки на уровне действий. Существует два уровня трассировки: базовая и подробная. При базовой трассировке отображется время начала и окончания каждого действия в последовательности runbook, а также сведения обо всех повторных действиях. Например, число попыток и время начала действия. Подробная трассировка предоставляет базовые возможности, а также запись в журнал входных и выходных данных каждого действия. 

В настоящее время при трассировке на уровне действий записи добавляются с помощью подробного потока. Поэтому при включении трассировки необходимо включить подробное ведение журнала. Для графических модулей runbook с включенной трассировкой вести записи о ходе выполнения не требуется. Базовая трассировка не только решает эту задачу, но и является более информативной.

![Представление потоков заданий графической разработки](media/automation-runbook-output-and-messages/job-streams-view-blade.png)

На приведенном выше изображении видно, что при включении подробного ведения журнала и трассировки графические последовательности runbook предоставляют гораздо больше данных в представлении **Потоки заданий** в рабочей среде. Эти дополнительные сведения могут потребоваться для устранения неполадок runbook в рабочей среде. 

Однако если эти данные не требуются для контроля за выполнением runbook в целях устранения неполадок, обычно трассировку рекомендуется отключить. Записи трассировки могут быть очень многочисленными. В зависимости от того, какая конфигурация трассировки выбрана (базовая или подробная), при трассировке графических последовательностей runbook создается от двух до четырех записей по каждому действию.

**Чтобы включить трассировку на уровне действий, сделайте следующе.**

1. На портале Azure выберите свою учетную запись службы автоматизации.
2. Щелкните **Модули Runbook** в разделе **Автоматизация процессов**, чтобы открыть список модулей runbook.
3. На странице "Модули Runbook" выберите графическую последовательность runbook.
4. В разделе **Параметры** щелкните **Ведение журналов и трассировка**.
5. На странице "Ведение журнала и трассировка" в разделе **Подробные записи в журнале** щелкните **Включить**, чтобы активировать запись подробных сведений в журнал.
6. В разделе **Трассировка уровня действия** измените уровень трассировки на **Базовый** или **Подробный** в зависимости от того, какой из них требуется.<br>

   ![Страница ведения журналов и трассировки графической разработки](media/automation-runbook-output-and-messages/logging-and-tracing-settings-blade.png)

### <a name="retrieve-runbook-output-and-messages-in-microsoft-azure-monitor-logs"></a>Получение выходных данных и сообщений runbook в журналах Microsoft Azure Monitor

Служба автоматизации Azure может отправлять состояние задания runbook и потоки заданий в рабочую область Log Analytics. Azure Monitor поддерживает журналы, которые позволяют:

* получить информацию о заданиях службы автоматизации;
* активировать отправку электронного сообщения или оповещения в соответствии с состоянием задания runbook (например, сбой или приостановка);
* создавать сложные запросы для потоков заданий;
* коррелировать задания и учетные записи службы автоматизации;
* визуализировать журнал заданий.

Дополнительные сведения о настройке интеграции с журналами Azure Monitor для сбора данных заданий, их корреляции и выполнения с ними соответствующих действий см. в статье [Пересылка состояния задания и потоков заданий из службы автоматизации в журналы Azure Monitor](automation-manage-send-joblogs-log-analytics.md).

## <a name="next-steps"></a>Дальнейшие действия

* Сведения о работе с последовательностями runbook см. в статье [Управление последовательностями runbook в службе автоматизации Azure](manage-runbooks.md).
* Дополнительные сведения о PowerShell см. в [документации PowerShell](/powershell/scripting/overview).
* * Справочник по командлетам PowerShell см. в документации по [Az.Automation](/powershell/module/az.automation/?view=azps-3.7.0#automation).
