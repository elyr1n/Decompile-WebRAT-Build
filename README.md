### Decompile-WebRAT-Build

---

### Описание файла

* Файл упакован с помощью UPX, но с помощью upx -d file.exe легко снять протектор UPX
* Все функции легко распознать с помощью GoReSym - активировать Python-файл в IDA и закинуть output.json чтобы переименовать функции в читабельный вид

---

### Функции и их описание (все взято с IDA и output.txt):

| Функция | Описание |
|---------|----------|
| `main.getDrives` | получение списка всех дисков |
| `main.getWebcams` | получение списка всех веб-камер |
| `main.getMics` | получение списка всех микрофонов |
| `main.getDevices` | получает информацию о классе устройства из системного реестра |
| `main.getClipboardText` | получение буфера обмена |
| `main.runKeylogger` | переключение работоспособности кейлоггера (сборщик всех нажатых клавиш) |
| `main.getScreen + main.sendScreen` | получение / отправка экрана мониторов |
| `main.IsIdle` | получение бездействия пользователя: проверка промежутка времени, в который пользователь ничего не вводил (используется для определения времени бездействия) |
| `main.getActiveWin + main.GetWindowText` | получение дескриптора окна с помощью main.getActiveWin и получение название окна с помощью main.GetWindowText |
| `main.GetHWID` | получение MachineGuid (HWID) с помощью реестра |
| `main.tonResolve + main.tryTonResolve` | проверяет запущен-ли процесс каждую секунду, и если нет - пересоздает его |
| `main.Steal` | полный запуск функций: логины/пароли (main.getGecko / main.getChrome / main.getYandexLogins), cookies, сессии (steam, telegram, discord / main.getSteams, main.getDiscord), скриншот, крипто-кошельки и упаковывает всё в .zip |

(функции не все, закинул самые основные в описание чтобы показать как работает этот ратник)

---

### Детали
* Ссылка на дизассемблерный файл и .json файл сигнатур для GoReSym: https://drive.google.com/file/d/1S-Pit76ttTK62xTlrJs_CpoQyOuRmfBA/view?usp=sharing
* Ссылка с VirusTotal: https://www.virustotal.com/gui/file/a01d84b8274b53fa29c2904457057dcd4c969a1d769eca103f5c9507091720fe
