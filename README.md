# asleep_scanner (patched fork)

Пропатченный форк [asleep_scanner](https://github.com/S0Ulle33/asleep_scanner) —
брутфорсер Dahua DVR/NVR/IP-камер на порту 37777 с поддержкой массового сканирования через masscan.


> **Disclaimer:** Инструмент предназначен исключительно для тестирования собственного оборудования или при наличии письменного разрешения владельца. Несанкционированный доступ к чужим устройствам незаконен.

---

## Что исправлено в этом форке

- **Зависимости** — оригинальный `requirements.txt` содержал конфликтующие и устаревшие версии пакетов, которые не устанавливались на современных системах. Переписан с нуля на основе реальных импортов кода.
- **Совместимость** — протестировано на Debian 13 / Python 3.11+.

---

## Требования

- Python 3.10+
- [masscan](https://github.com/robertdavidgraham/masscan)

### Установка masscan

```bash
sudo apt install masscan

# Рекомендуется: дать права на raw-сокеты без sudo
sudo setcap cap_net_raw,cap_net_admin+eip $(which masscan)
```

---

## Установка

```bash
git clone https://github.com/qq23k/asleep-scanner.git
cd asleep-scanner

python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

## Использование

```
python3 asleep.py [-h] [-s SCAN_FILE] [-p PORTS] [-b BRUTE_FILE] [-l] [-m]
                 [-t THREADS] [--masscan-resume] [--no-snapshots] [--no-xml]
                 [--dead] [--country] [--random-country] [-d]
```

| Флаг | Описание |
|------|----------|
| `-s SCAN_FILE` | Файл с IP-диапазонами для сканирования |
| `-p PORTS` | Порты (по умолчанию: 37777) |
| `-b BRUTE_FILE` | Файл с IP для брутфорса (любой формат) |
| `-l` | Использовать `logins.txt` + `passwords.txt` вместо `combinations.txt` |
| `-m` | Запустить masscan и брутить результаты |
| `-t THREADS` | Количество потоков masscan (по умолчанию: 3000) |
| `--masscan-resume` | Продолжить прерванное сканирование |
| `--no-snapshots` | Не делать скриншоты |
| `--no-xml` | Не создавать файл SmartPSS XML |
| `--dead` | Записать не взломанные камеры в `dead_cams.txt` |
| `--country` | Сканировать по стране |
| `--random-country` | Сканировать по случайной стране |
| `-d` | Debug-режим (подробный вывод) |

### Примеры

```bash
# Сканировать диапазон и брутить
python3 asleep.py -m -s ips.txt

# Только брутфорс по готовому списку IP
python3 asleep.py -b targets.txt -p 37777 -d

# Кастомные порты
python3 asleep.py -b targets.txt -p 37777,37778,47777

# Сканировать по стране
python3 asleep.py --country

# Непрерывный режим (случайные страны)
python3 nonstop.py
```

---

## Результаты

После завершения результаты сохраняются в `reports/<дата-время>/`:

| Файл | Содержимое |
|------|-----------|
| `results_<timestamp>.csv` | `ip,port,login,password,channels,model` |
| `ips_<timestamp>.txt` | Список всех найденных IP:port |
| `save.xml` | Конфиг для SmartPSS |
| `tmp_snapshots/` | Скриншоты с каждого канала |

---

## Просмотр камер

- **Windows / macOS** — [SmartPSS](https://dahuawiki.com/SmartPSS)
- **Linux** — [TaniDVR](http://tanidvr.sourceforge.net/)

---

## GUI-оболочка

Этот форк используется как бэкенд для **IoT Recon** — GUI-приложения, 
которое объединяет masscan и asleep в один интерфейс с автопилотом:

👉 [IoT Recon](https://github.com/qq23k/ipdoor)

---

## Оригинальный проект

Форк основан на [asleep_scanner](https://github.com/S0Ulle33/asleep_scanner).
