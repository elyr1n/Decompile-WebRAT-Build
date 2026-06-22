## Описание файла

* Файл упакован с помощью UPX, но с помощью `upx -d file.exe` легко снять упаковщик UPX
* Все функции легко распознать с помощью GoReSym - активировать Python-файл в IDA и закинуть output.json чтобы переименовать функции в читабельный вид

---

## Основная работа подготовки программы к работе (кратко)

1. Ратник создаёт ZIP-архив, и создает там файл UserInformation.txt, куда кладёт то время, в которое был запущен ратник.
2. Сохраняет размер экранов (salat_screenshot_GetDisplayBounds | используется для панели, чтобы ходить по мониторам).
3. Просматривает и сохраняет профили Mozilla Firefox если они есть (LocalCache\\Roaming\\Mozilla\\Firefox\\Profiles).
4. Смотрит путь где находится Telegram с помощью реестра (HKEY_CURRENT_USER\\SOFTWARE\\Classes\\tg\\shell\\open\\command) и сохраняет папку tdata если она есть для входа в аккаунты (Clients\\tdata).
5. Сохраняет Discord-токены для входа в аккаунты (Clients\\DiscordTokens.txt).
6. Сохраняет Steam-токены для входа в аккаунты (Clients\\SteamTokens.txt).
7. Сохраняет из каждого браузера Token, Cookies, Autofills, Logins (Browsers\\Token_ | Browsers\\Cookies_ | Browsers\\Autofills_ | Browsers\\Logins_), при этом получая Master-Key (main.GetChromiumMasterKeys | main.GetGeckoMasterKey).
8. ZIP-архив в котором сохранена информация скачивается с панели пользователя кто создавал билд ратника, и от конкретного пользователя от которого он запросил информацию.

---

## Кража крипто-кошельков

RAT целенаправленно охотится за крипто-кошельками (не все, самые популярные и основные):

| Кошелек | Способ кражи |
|---------|--------------|
| MetaMask2 | Кража seed-фраз и приватных ключей |
| TonKeeper | Кража через TON-адреса |
| SuiWallet | Кража данных кошелька |
| Liquality | Кража мнемонической фразы |
| Maiar DEFI | Кража данных DeFi-кошелька |

---

## Функции и их описание

| Функция | Описание |
|---------|----------|
| `main.getDrives` | Получение списка всех дисков |
| `main.getWebcams` | Получение списка всех веб-камер |
| `main.getMics` | Получение списка всех микрофонов |
| `main.getDevices` | Получение информации о классе устройства из системного реестра |
| `main.getClipboardText` | Получение буфера обмена |
| `main.runKeylogger` | Сборщик всех нажатых клавиш |
| `main.getScreen + main.sendScreen` | Получение / отправка скриншотов мониторов |
| `main.IsIdle` | Проверка времени бездействия пользователя |
| `main.getActiveWin + main.GetWindowText` | Получение заголовка активного окна |
| `main.GetHWID / main_GetHWID2` | Получение MachineGuid (HWID) из реестра |
| `main.tonResolve + main.tryTonResolve` | **Получение C2-адресов через TON DNS (The Open Network)** |
| `main.executeCommand / main.shellCommand` | Запуск программ / выполнение shell-кода |
| `main.getRandomFolders / main.getRandomProcesses` | Поиск случайных папок и процессов для маскировки RAT |
| `main.Steal / main.main` | Сбор логинов/паролей, cookies, сессий, скриншотов, крипто-кошельков и упаковка в .zip / подготовка программы к работе |

---

## C2 (Command & Control) коммуникация

RAT использует **децентрализованную сеть TON** для получения адресов C2:

1. В бинарнике хранятся **RSA-зашифрованные TON-адреса** (4 бэкапа)
2. При запуске RAT расшифровывает их и делает DNS-запрос к TON
3. TON DNS возвращает JSON с реальными C2-адресами
4. RAT подключается к полученному C2 и ждет команд

**Смена C2:** Если текущий C2 недоступен, RAT переключается на следующий из списка. Всего 4 бэкап-адреса.

---

## Маскировка

Найден JSON со списком системных процессов, под которые маскируется RAT:

```json
[
    "explorer.exe",
    "svchost.exe",
    "smss.exe",
    "csrss.exe",
    "services.exe",
    "lsass.exe",
    "taskhostw.exe",
    "taskhost.exe",
    "audiodg.exe",
    "wininit.exe",
    "spoolsv.exe",
    "dwm.exe"
]
```

**Признак заражения:** Если вы видите `explorer.exe` или `svchost.exe`, запущенный НЕ из `C:\Windows\System32`, а из `Temp` или `AppData` — это RAT.

---

## Дополнительные файлы

* Дизассемблерный файл и .json сигнатур для GoReSym:  
  https://drive.google.com/file/d/1S-Pit76ttTK62xTlrJs_CpoQyOuRmfBA/view?usp=sharing

* VirusTotal:  
  https://www.virustotal.com/gui/file/a01d84b8274b53fa29c2904457057dcd4c969a1d769eca103f5c9507091720fe
