---
permalink: /update-to-latest.html
title: Безопасное обновление прошивки 
author_profile: true
---
{% include toc title="Разделы" %}

{% capture notice-5 %}
## Выберите эксплойт, который используете для активации взлома 

* **Fusée Gelée** подразумевает взлом через аппаратную уязвимость. Если для запуска прошивки вы используете {% include abbr/host.txt abbr="хост" %}, то у вас определённо Fusée Gelée
* **Caffeine** подразумевает взлом через программную уязвимость в браузере. Если для запуска взлома вы запускаете браузер, то у вас определённо Caffeine

<div class="select_fw__wrapper">
	<div class="select_fw">
	    <input type="radio" id="button1" name="exploit" value="button1" checked />
	    <label for="button1">Fusée Gelée</label>
	    <input type="radio" id="button2" name="exploit" value="button2" />
	    <label for="button2">Caffeine</label>
	</div>
</div>

<!--В данный момент последней прошивкой является {% include /vars/sys_version.txt %}, однако, поскольку она только вышла и недостаточно оттестирована, а так же потому что ещё нет игр требующих её, не рекомендуется пока на неё обновляться. Если же вы уже на ней установите рекомендуемую прошивку по инструкции ниже!
{: .notice--warning}-->

{% spoiler Видеоинструкция %}

{% include youtube.html id="I6R_XaYt7ps" %}
{: .text-center}
{: .notice--info}

{% endspoiler %}
{% endcapture %}
<div id="selector">{{ notice-5 | markdownify }}</div>

## Важная информация

Это же руководство подходит и для понижения прошивки. Помните, что в случае понижения прошивки есть шанс, что пониженная прошивка не запустится. В таком случае вам будет необходимо [восстанавливать прошивку вручную](downgrade_fw){:target="_blank"}. 

## Теоретическая часть

Если вы хотите получить поддержку {% include abbr/exfat.txt %} или {% include abbr/sdhc.txt %}, полностью следуйте этому руководству, даже если у вас и так уже последняя прошивка. Действия для активации {% include abbr/exfat.txt %}, {% include abbr/sdhc.txt %} и для поднятия прошивки идентичный! Да,нужно делать, даже если у вас уже и так {% include /vars/update_version.txt %}
{: .notice--warning}

При прошивке на некоторые версии (1.0.0, 2.0.0, 5.0.0, 6.0.0, 6.2.0, 7.0.0) приставка сжигает специальные предохранители на чипе, чтобы по их состоянию отслеживать версию системного ПО на вашей приставке. В обычном режиме при установке прошивки, консоль проверяет количество сожжённых предохранителей. Если это количество больше, чем требуется для прошивки, то консоль понимает, что вы пытаетесь установить прошивку ниже, чем была установлена до этого, и не позволит это сделать. Если же количество сожжённых предохранителей ниже, чем требует устанавливаемая прошивка, то прошивка разрешается и в ходе неё сжигается столько предохранителей сколько необходимо, чтобы соответствовать указанному в прошивке количеству. Таким образом, если вы обновились до {% include /vars/update_version.txt %} официально, у вас на приставке будет сожжено 12 предохранителей. Если вы попытаетесь восстановить бекап, скажем, сделанный на прошивке 8.1.0, для работы которой требуется 10 предохранителей (а у вас сожжено 12), то приставка поймёт это и официальная прошивка не запустится!

**Количество сожжённых предохранителей не важно для кастомных прошивок. Вы сможете откатиться на любую прошивку при условии использования загрузчика hekate.** Официальная же прошивка не запустится, если версия установленной прошивки ниже, чем на то указывают предохранители. Если выше, то OFW сожжёт предохранители, чтобы они соответствовали запущенной версии прошивки.

По этой инструкции можно менять версию своего ПО на любую, а не только на {% include /vars/update_version.txt %}. 

Другие версии ПО Switch можно найти [здесь](https://darthsternie.net/index.php/switch-firmwares/){:target="_blank"} 
{: .notice--info}

## Что понадобится

* Умение [запускать пейлоады через Fusée Gelée](fusee-gelee){:target="_blank"} и [{% include abbr/cfw.txt abbr="кастомную прошивку" %}](cfw){:target="_blank"}
* Свежая версия {% include abbr/kefir_addr.txt %}
* Прошивка {% include /vars/update_version.txt %} (скачайте по любой из ссылок):
	* [magnet](magnet:?xt=urn:btih:B23F438BA3FED6462C067496F14D37D88B46BEF8&dn=10.1.0.zip&tr=udp%3a%2f%2ftracker.openbittorrent.com%3a80%2fannounce){:target="_blank"}
	* [ЯД](https://yadi.sk/d/BzsYZLjroSgLjw){:target="_blank"}
	* [GD](https://drive.google.com/file/d/10L8zKy-Js9L3QyljOqBJLz-zTXDTxofO/view?usp=sharing){:target="_blank"}
	* [MEGA](https://mega.nz/file/Bt1h3QpD#oCQVvmE57c_RNveZIl1wVZ30ktposLsUrUoEMIgcKk8){:target="_blank"}
* Карта памяти 

## Инструкция

### Часть I - Резервное копирование NAND

Пропустите эту часть, если у вас уже есть резервная копия!

Если резервной копии нет, эту часть нужно делать **обязательно**!
{: .notice--danger}

1. Создайте [резервную копию NAND](backup-nand){:target="_blank"} консоли и поместите её в надёжное место 

### Часть II - Подготовка к обновлению
Важная информация!! Здесь нужно дать разъяснения по поводу работы файловой системы в Switch. Единственная ФС, которая изначально поддерживается - это [FAT32](http://customfw.xyz/format_sd){:target="_blank"}. Для поддержки exFAT на приставку нужно ставить драйвера. Запрос на установку драйверов будет только в том случае, если приставка увидит, что в консоль вставлена карта в формате exFAT, но это на официальной прошивке. Кастомная же просто не загрузится. После логотипа Nintendo Switch при загрузке Atmosphere вы просто упрётесь в чёрный экран. С помощью безопасного обновления прошивки мы можем установить эти драйвера принудительно, но только в том случае, если для обновления мы будем использовать карту памяти в [FAT32](http://customfw.xyz/format_sd){:target="_blank"} или у нас уже установлены нужные драйвера. Если вы впервые взламываете консоль то у вас, вероятнее всего, не установлены нужные драйвера. Можете просто использовать любую другую карту в [FAT32](http://customfw.xyz/format_sd){:target="_blank"}, 4ГБ хватит с головой для обновления. После обновления, карты exFAT будут работать. Устанавливайте драйвера exFAT при безопасном обновлении даже в том случае, если планируете использовать только FAT32! 
{: .notice--warning}

1. Выключите Switch и вставьте его карту памяти в ПК 
	* **Внимание!** Перед обновлением удалите следующие папки с карты памяти, если таковые имеются:
		* `sxos/titles/0100000000001000`
		* `sxos/titles/0100000000001013`
		* `sxos/titles/0100000000000352`<br>
		или
		* `atmosphere/contents/0100000000001000`
		* `atmosphere/contents/0100000000001013`
		* `atmosphere/contents/0100000000000352`
1. Установите `.7z`-архив {% include abbr/kefir_addr.txt %}, если ещё не делали этого
	* Если версия вашего системного ПО 1.0.0, удалите папки `\atmosphere\exefs_patches` и `\atmosphere\kip_patches\fs_patches`:
		* После того, как вы удачно обновите системное ПО, обязательно **ещё раз установите {% include abbr/kefir_addr.txt %}**
1. Скопируйте **содержимое** архива `{% include /vars/update_version.txt %}.zip` с прошивкой в корень карты памяти
	* Ещё раз - **содержимое** архива **с прошивкой**. Не архив с кефиром, **с прошивкой**! Архив с прошивкой расположен в части [Что понадобится](#что-понадобится). Нужно скопировать не сам архив, а именно его **содержимое**
	* Вместо этого вы можете скачать архив со свежей версией системного ПО через приложение **FreshHay** прямо на вашей приставке. Для этого запустите [HBL](hbl){:target="_blank"} через игру, выберите приложение **FreshHay** и нажмите (A), чтобы начать закачку прошивки. Программа сама скачает последнюю версию системного ПО в папку `switch/FreshHay`. После, распакуйте скачанный архив с помощью NXShell 
1. Вставьте карту памяти обратно в приставку

### Часть III - Обновление прошивки

{% capture notice-1 %}

<!-- Fusée Gelée -->
1. Запустите [{% include abbr/cfw.txt abbr="кастом" %}](cfw){:target="_blank"}
	* Если вы собираетесь обновлять {% include abbr/sysnand.txt abbr="SysNAND" %}, то его и запускайте. Если собираетесь обновлять {% include abbr/emunand.txt abbr="EmuNAND" %},  то запускайте его
	* Если после логотипа Nintendo вы наблюдаете чёрный экран, значит вы проигнорировали [форматирование карты памяти в FAT32](http://customfw.xyz/format_sd){:target="_blank"}, которое нужно было сделать выше
1. Откройте [Homebrew Launcher](hbl){:target="_blank"} **в режиме апплета**
	* Запускайте только в режиме апплета, иначе программа будет вылетать. Что такое режим апплета описано в инструкции по запуску [Homebrew Launcher](hbl){:target="_blank"}
1. Запустите **ChoiDujourNX**
1. Перейдите в папку, в которой находится прошивка {% include /vars/update_version.txt %}, скопированная ранее, и нажмите "**Choose**"

	![]({{ base_path }}/images/screenshots/choi/fw.png)
	{: .text-center}

	* Если вы не можете перейти в папку (папка отображается как файл), значит ваша папка имеет неверные атрибуты (чаще всего архивный). Как их исправить описано в [FAQ](faq){:target="_blank"} или разделе [проблемы и их решения](troubleshooting){:target="_blank"}
	
	![]({{ base_path }}/images/screenshots/choi/choose.png)
	{: .text-center}

1. Выберите "**{% include /vars/update_version.txt %} (exFAT)**"

	![]({{ base_path }}/images/screenshots/choi/exfat.png)
	{: .text-center}

1. Дождитесь окончания обработки прошивки и нажмите "**Select firmware**"
	
	![]({{ base_path }}/images/screenshots/choi/select_fw.png)
	{: .text-center}

1. Дождитесь окончания распаковки прошивки и нажмите "**Start installation**"
	
	![]({{ base_path }}/images/screenshots/choi/start.png)
	{: .text-center}

	* При установки прошивки с такой же версией, как исходная (например, у вас стоит {% include /vars/update_version.txt %}, а вы пытаетесь установить {% include /vars/update_version.txt %} (exFAT)) может возникать ошибка. В таком случае, вам придётся установить прошивку с версией ниже (например, если у вас сейчас стоит 10.0.2, поставьте 9.2.0), а уже потом повышать её. 
1. После окончания установки нажмите "**Reboot**", "**Shutdown**", чтобы выключить приставку 

Вы можете удалить папку с прошивкой {% include /vars/update_version.txt %} с карты памяти. Она нам больше не нужна
{: .notice--info}

### Часть IV - AutoRCM

AutoRCM - на консоли специальным образом изменяется загрузочный сектор, вследствие чего консоль не может загрузиться прямо в систему и загружается автоматически в режим RCM. Достаточно просто включить консоли и она автоматически попадёт в режим восстановления. Не нужно зажимать комбинацию кнопок и использовать замыкатель, но пейлоад для запуска прошивки всё равно передавать нужно!
{: .notice--info}

После обновления через ChoiDujourNX, AutoRCM будет включён автоматически. Теперь приставка всегда при включении будет переходить в режиме RCM. 
{% endcapture %}
<div class="hideble button1">{{ notice-1 | markdownify }}</div>

{% capture notice-2 %}

<!-- Caffeine -->
1. Запустите [{% include abbr/cfw.txt abbr="кастом" %}](cfw){:target="_blank"}
	* Если вы собираетесь обновлять {% include abbr/sysnand.txt abbr="SysNAND" %}, то его и запускайте. Если собираетесь обновлять {% include abbr/emunand.txt abbr="EmuNAND" %},  то запускайте его
	* Если после логотипа Nintendo вы наблюдаете чёрный экран, значит вы проигнорировали [форматирование карты памяти в FAT32](http://customfw.xyz/format_sd){:target="_blank"}, которое нужно было сделать выше
1. **Обязательно убедитесь, что находитесь в EmuNAND**, если поднимаете версию вашего системного ПО. Для этого перейдите в **Системные настройки** -> **Система**. В строке ниже **Обновления системы** будет написана версия вашего системного ПО, версия Atmosphere и буква **S** или **E**, где S будет означать SysNAND, а E - EmuNAND. 
	* На SX OS вы не сможете проверить этим способом запущен ли EmuNAND, поскольку в SX OS там будет написана только версия системного ПО. Если вы используете SX OS, проверьте в загрузчике активирован ли EmuNAND
	* Если вы случайно обновите SysNAND до последней версии системного ПО, то любая загрузка в официальную прошивку сожжёт вам предохранители и вы не сможете прошить приставку! Это относится только к методу взлома через Caffeine.

	![]({{ base_path }}/images/screenshots/choi/sysnand.png)
	{: .text-center}

1. Откройте [Homebrew Launcher](hbl){:target="_blank"} в режиме апплета 
1. Запустите **ChoiDujourNX**
1. Перейдите в папку, в которой находится прошивка {% include /vars/update_version.txt %}, скопированная ранее, и нажмите "**Choose**"

	![]({{ base_path }}/images/screenshots/choi/fw.png)
	{: .text-center}

	* Если вы не можете перейти в папку (папка отображается как файл), значит ваша папка имеет неверные атрибуты (чаще всего архивный). Как их исправить описано в [FAQ](faq){:target="_blank"} или разделе [проблемы и их решения](troubleshooting){:target="_blank"}

	![]({{ base_path }}/images/screenshots/choi/choose.png)
	{: .text-center}

1. Выберите "**{% include /vars/update_version.txt %} (exFAT)**"

	![]({{ base_path }}/images/screenshots/choi/exfat.png)
	{: .text-center}

1. Дождитесь окончания обработки прошивки и нажмите "**Select firmware**"
	
	![]({{ base_path }}/images/screenshots/choi/select_fw.png)
	{: .text-center}

{% capture notice-3 %}
{: start="8"}
1. **ВНИМАНИЕ!** При прошивке через [Caffeine](caffeine){:target="_blank"} **обязательно** убедитесь что в поле "**Target firmware**" находится прошивка "**{% include /vars/update_version.txt %} (exFAT)**", перед строкой "**Prevent fuse burning (enable AutoRCM)**" стоит **крестик**! Если всё так, то нажимайте **Start installation**
	* Если там стоит галочка, нажмите на неё, в появившемся окне выберите **Yes, I am sure**
	* При установки прошивки с такой же версией, как исходная (например, у вас стоит {% include /vars/update_version.txt %}, а вы пытаетесь установить {% include /vars/update_version.txt %} (exFAT)) может возникать ошибка. В таком случае, вам придётся установить прошивку с версией ниже (например, если у вас сейчас стоит 10.0.2, поставьте 9.2.0), а уже потом повышать её. 

![]({{ base_path }}/images/screenshots/choi/noautorcm.png)
{: .text-center}
{% endcapture %}
<div class="notice--danger">{{ notice-3 | markdownify }}</div>

{: start="9"}
1. После окончания установки нажмите "**Reboot**", "**Reboot**", чтобы выключить приставку 
1. Во время появления сплешскрина {% include abbr/kefir_addr.txt %} нажмите (VOL-) (кнопка понижения громкости), чтобы попасть в hekate

Вы можете удалить папку с прошивкой {% include /vars/update_version.txt %} с карты памяти. 
{: .notice--info}

Вы можете отформатировать карту памяти обратно в exFAT через ПК
{: .notice--info}

### Часть IV - AutoRCM

AutoRCM - на консоли специальным образом изменяется загрузочный сектор, вследствие чего консоль не может загрузиться прямо в систему и загружается автоматически в режим RCM. Достаточно просто включить консоли и она автоматически попадёт в режим восстановления. Не нужно зажимать комбинацию кнопок и использовать замыкатель, но пейлоад для запуска прошивки всё равно передавать нужно!
{: .notice--info}

Для приставок прошитых через Caffeine, использовать AutoRCM **НЕЛЬЗЯ**! Это **гарантированно** приведёт к брику приставки! 
{: .notice--danger}

1. Находясь в {% include abbr/hekate.txt abbr="hekate" %} перейдите в раздел **Tools**
1. Выберите раздел **Arch bit • RCM • Touch • Partitions**, находящийся внизу экрана справа
1. Убедитесь, что возле **AutoRCM** стоит **OFF**
	* Если это не так, нажмите на **AutoRCM**, затем **OK**


{% endcapture %}
<div class="hideble button2 hide">{{ notice-2 | markdownify }}</div>

## Важно знать

* Даже один единственный запуск приставки в официальную прошивку не через {% include abbr/hekate.txt abbr="hekate" %} сожжёт предохранители, которые мы пытались сохранить 
* Если после прошивки вам предложат обновить контроллеры - обновляйте
* Если после обновления у вас чёрный экран после логотипа Nintendo, обновите {% include abbr/kefir_addr.txt %}
	* Если не помогло, прочтите внимательно разделы [Проблемы и их решения](troubleshooting){:target="_blank"} и [FAQ](faq){:target="_blank"}
* Если приставка не загружается после прошивки (падает с ошибкой, например), [восстановите бекап](backup-nand#%D0%B2%D0%BE%D1%81%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5-%D1%80%D0%B5%D0%B7%D0%B5%D1%80%D0%B2%D0%BD%D0%BE%D0%B9-%D0%BA%D0%BE%D0%BF%D0%B8%D0%B8){:target="_blank"} или [восстанавливайте прошивку вручную](downgrade_fw){:target="_blank"}

___

[Закрыть страницу](javascript:window.close();)
{: .notice--success}