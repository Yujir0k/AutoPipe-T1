# AutoPipe-T1 - Автоматическая генерация CI/CD пайплайнов

![Python Version](https://img.shields.io/badge/python-3.10%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)

**AutoPipe-T1** — высокоточный CLI-инструмент для автоматической генерации CI/CD пайплайнов. Анализирует Git-репозитории и создает оптимизированные конфигурации `.gitlab-ci.yml` с интеграцией SonarQube, Nexus и Kubernetes.

---

## 🏆 Хакатон T1 Москва

**AutoPipe-T1** — решение команды **R² negative** для хакатона **T1. Москва** в задаче **«Self-Deploy: CI/CD без DevOps»**. Команда заняла **4 место**, а проект сфокусирован на автоматизации генерации и запуска CI/CD pipeline без ручной DevOps-настройки.

- [Сертификат финалиста](docs/hackathon/t1-moscow-2025/autopipe-t1-certificate.pdf)
- [Скачать презентацию проекта](docs/hackathon/t1-moscow-2025/autopipe-t1-self-deploy-presentation.pptx)
- [Смотреть демо-видео](docs/hackathon/t1-moscow-2025/autopipe-t1-demo.mp4)

<details>
<summary><strong>Листать презентацию прямо в README</strong></summary>

<p align="center">
  <img src="docs/hackathon/t1-moscow-2025/slides/slide-01.png" alt="AutoPipe-T1 presentation slide 01" width="800"/>

  <img src="docs/hackathon/t1-moscow-2025/slides/slide-02.png" alt="AutoPipe-T1 presentation slide 02" width="800"/>

  <img src="docs/hackathon/t1-moscow-2025/slides/slide-03.png" alt="AutoPipe-T1 presentation slide 03" width="800"/>

  <img src="docs/hackathon/t1-moscow-2025/slides/slide-04.png" alt="AutoPipe-T1 presentation slide 04" width="800"/>

  <img src="docs/hackathon/t1-moscow-2025/slides/slide-05.png" alt="AutoPipe-T1 presentation slide 05" width="800"/>

  <img src="docs/hackathon/t1-moscow-2025/slides/slide-06.png" alt="AutoPipe-T1 presentation slide 06" width="800"/>

  <img src="docs/hackathon/t1-moscow-2025/slides/slide-07.png" alt="AutoPipe-T1 presentation slide 07" width="800"/>

  <img src="docs/hackathon/t1-moscow-2025/slides/slide-08.png" alt="AutoPipe-T1 presentation slide 08" width="800"/>

  <img src="docs/hackathon/t1-moscow-2025/slides/slide-09.png" alt="AutoPipe-T1 presentation slide 09" width="800"/>

  <img src="docs/hackathon/t1-moscow-2025/slides/slide-10.png" alt="AutoPipe-T1 presentation slide 10" width="800"/>

</p>

</details>

## Содержание

- [Возможности](#-возможности)
- [Требования](#-требования)
- [Быстрый старт](#-быстрый-старт)
- [Полная установка](#-полная-установка)
- [Использование](#-использование)
- [Поддерживаемые технологии](#-поддерживаемые-технологии)
- [Архитектура](#-архитектура)
- [Устранение неполадок](#-устранение-неполадок)

---

## 🌟 Возможности 

### Автоматическое определение стека
- Поддержка **6 языков**: Python, Node.js/TypeScript, Java/Kotlin, Go, PHP, .NET
- Определение фреймворков: Django, FastAPI, Express, NestJS, Spring Boot, Gin и др.
- Автоматический выбор build tool и версий

### Полный CI/CD Pipeline
- **7 стадий**: build → test → analyze → package → publish → deploy_staging → deploy_production
- Multi-stage Docker builds с кэшированием
- Автоматическая генерация coverage reports

### Интеграции
- **SonarQube** - статический анализ качества кода
- **Nexus Repository** - публикация пакетов (PyPI, npm, Maven)
- **GitLab Container Registry** - Docker образы
- **Kubernetes** - деплой в staging/production

---

## 📋 Требования

### Минимальные системные требования

| Компонент | Требование |
|-----------|------------|
| ОС | Windows 10/11, Linux, macOS |
| RAM | **8 GB** (рекомендуется 16 GB для GitLab) |
| Disk | 50 GB свободного места |
| CPU | 4 ядра |

### Необходимое ПО

| ПО | Версия | Ссылка |
|----|--------|--------|
| Docker Desktop | 24+ | https://www.docker.com/products/docker-desktop |
| Python | 3.10+ | https://www.python.org/downloads/ |
| Git | 2.30+ | https://git-scm.com/downloads |
| k3d (опционально) | 5.6+ | https://k3d.io/ |

---

## 🚀 Быстрый старт

### Шаг 1: Клонирование репозитория

```bash
# ВАЖНО: Используйте ветку master для актуальной версии
git clone -b master https://gateway-codemetrics.saas.sferaplatform.ru/app/sourcecode/api/team43/T1.git
cd T1
```

### Шаг 2: Автоматическая установка (рекомендуется)

Скрипты автоматически установят все зависимости: Python, Git, Docker.

**Windows (PowerShell):**
```powershell
.\install.ps1
```

**Windows (CMD):**
```cmd
install.bat
```

**Linux / macOS:**
```bash
chmod +x install.sh
./install.sh
```

### Шаг 3: Запуск CI/CD инфраструктуры

```bash
# Запуск всех сервисов (GitLab, SonarQube, Nexus, Runner)
docker-compose up -d

# Проверка статуса
docker-compose ps
```

⏳ **Важно:** GitLab требует 3-5 минут для инициализации!

### Шаг 4 (альтернатива): Ручная установка

Если скрипт автоматической установки не подходит:

```bash
# Создание виртуального окружения
python -m venv venv

# Активация (Windows)
venv\Scripts\activate

# Активация (Linux/macOS)
source venv/bin/activate

# Установка зависимостей
pip install -e .
```

### Шаг 5: Деплой первого проекта

```bash
python -m autopipe deploy https://github.com/gin-gonic/gin.git \
    -t glpat-autopipe2025newtoken \
    --sonar-token squ_40c7ee83cdc698a5ab20d46bdf7d73e12771b414 \
    --name my-first-project
```

### Шаг 5: Проверка результатов

| Сервис | URL | Логин | Пароль |
|--------|-----|-------|--------|
| GitLab | http://localhost:8080 | root | см. credentials.txt |
| SonarQube | http://localhost:9000 | admin | Qwerty123 |
| Nexus | http://localhost:8081 | admin | Qwerty123 |

---

## 📦 Полная установка

### 1. Установка Docker Desktop

#### Windows
```powershell
# Скачайте и установите Docker Desktop
# https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe

# После установки перезагрузите компьютер
# Запустите Docker Desktop

# Проверьте установку
docker --version
docker-compose --version
```

#### Linux (Ubuntu/Debian)
```bash
# Установка Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
newgrp docker

# Установка Docker Compose
sudo apt-get install docker-compose-plugin

# Проверка
docker --version
docker compose version
```

#### macOS
```bash
# Через Homebrew
brew install --cask docker

# Или скачайте с docker.com
# После установки запустите Docker Desktop

# Проверка
docker --version
```

### 2. Установка Python

#### Windows
1. Скачайте Python 3.11+: https://www.python.org/downloads/
2. **Важно:** При установке поставьте галочку "Add Python to PATH"
3. Перезапустите терминал

```bash
# Проверка
python --version
pip --version
```

#### Linux
```bash
sudo apt-get update
sudo apt-get install python3.11 python3.11-venv python3-pip
```

#### macOS
```bash
brew install python@3.11
```

### 3. Клонирование и настройка

```bash
# Клонирование
git clone https://gateway-codemetrics.saas.sferaplatform.ru/app/sourcecode/api/team43/T1.git
cd T1

# Создание виртуального окружения (рекомендуется)
python -m venv venv

# Активация виртуального окружения
# Windows:
venv\Scripts\activate
# Linux/macOS:
source venv/bin/activate

# Установка AutoPipe
pip install -e .
```

### 4. Запуск Docker-инфраструктуры

```bash
# Запуск всех сервисов
docker-compose up -d

# Ожидание готовности (3-5 минут)
# Проверка статуса GitLab
docker exec t1-gitlab-1 gitlab-ctl status
```

### 5. Установка k3d (для Kubernetes деплоя)

#### Windows (PowerShell)
```powershell
# Через Chocolatey
choco install k3d

# Или скачайте напрямую
Invoke-WebRequest -Uri "https://github.com/k3d-io/k3d/releases/download/v5.6.0/k3d-windows-amd64.exe" -OutFile "k3d.exe"
```

#### Linux
```bash
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

#### macOS
```bash
brew install k3d
```

#### Создание кластера
```bash
# Создание кластера
k3d cluster create autopipe-cluster --servers 1 --agents 2

# Создание namespaces
kubectl create namespace staging
kubectl create namespace production

# Проверка
kubectl get nodes
kubectl get namespaces
```

---

## 💻 Использование

### Основная команда

```bash
python -m autopipe deploy <URL_РЕПОЗИТОРИЯ> -t <GITLAB_TOKEN> [OPTIONS]
```

### Параметры

| Параметр | Описание | Обязательный | По умолчанию |
|----------|----------|--------------|--------------|
| `URL` | URL Git репозитория или локальный путь | Да | - |
| `-t, --token` | GitLab API токен | Да | - |
| `--sonar-token` | SonarQube токен | Нет | - |
| `--name` | Имя проекта в GitLab | Нет | Из URL |
| `--no-wait` | Не ждать завершения pipeline | Нет | False |
| `--timeout` | Таймаут ожидания (секунды) | Нет | 600 |

### Примеры использования

#### Python проект
```bash
python -m autopipe deploy https://github.com/encode/httpx.git \
    -t glpat-autopipe2025newtoken \
    --sonar-token squ_40c7ee83cdc698a5ab20d46bdf7d73e12771b414 \
    --name python-httpx
```

#### Node.js проект
```bash
python -m autopipe deploy https://github.com/expressjs/express.git \
    -t glpat-autopipe2025newtoken \
    --sonar-token squ_40c7ee83cdc698a5ab20d46bdf7d73e12771b414 \
    --name nodejs-express
```

#### Go проект
```bash
python -m autopipe deploy https://github.com/gin-gonic/gin.git \
    -t glpat-autopipe2025newtoken \
    --sonar-token squ_40c7ee83cdc698a5ab20d46bdf7d73e12771b414 \
    --name go-gin
```

#### Java проект
```bash
python -m autopipe deploy https://github.com/spring-projects/spring-petclinic.git \
    -t glpat-autopipe2025newtoken \
    --sonar-token squ_40c7ee83cdc698a5ab20d46bdf7d73e12771b414 \
    --name java-spring
```

#### Локальный проект
```bash
python -m autopipe deploy ./my-project \
    -t glpat-autopipe2025newtoken \
    --name my-project
```

#### Быстрый деплой (без ожидания)
```bash
python -m autopipe deploy https://github.com/fastapi/fastapi.git \
    -t glpat-autopipe2025newtoken \
    --name fastapi-test \
    --no-wait
```

---

## 🔍 Поддерживаемые технологии

### Языки и фреймворки

| Язык | Версии | Фреймворки | Build Tools |
|------|--------|------------|-------------|
| Python | 3.8 - 3.13 | Django, FastAPI, Flask, Celery | Poetry, PIP, Pipenv, UV |
| Node.js | 18, 20, 22 | Express, NestJS, Koa, Fastify | npm, yarn, pnpm |
| TypeScript | - | Next.js, Angular, Vue, React, Nuxt | npm, yarn, pnpm |
| Java | 11, 17, 21 | Spring Boot, Quarkus, Micronaut | Maven, Gradle |
| Kotlin | - | Spring Boot, Ktor | Maven, Gradle |
| Go | 1.20 - 1.22 | Gin, Echo, Fiber, Chi | Go Modules |
| PHP | 8.1 - 8.3 | Laravel, Symfony | Composer |
| .NET | 6, 7, 8 | ASP.NET Core, Blazor | dotnet CLI |

### Coverage инструменты

| Язык | Инструмент | Формат отчета | SonarQube параметр |
|------|------------|---------------|-------------------|
| Python | pytest-cov | coverage.xml | sonar.python.coverage.reportPaths |
| Node.js | Jest/nyc | lcov.info | sonar.javascript.lcov.reportPaths |
| Java | JaCoCo | jacoco.xml | sonar.coverage.jacoco.xmlReportPaths |
| Go | go test | coverage.out | sonar.go.coverage.reportPaths |
| .NET | Coverlet | cobertura.xml | sonar.cs.opencover.reportsPaths |
| PHP | PHPUnit | clover.xml | sonar.php.coverage.reportPaths |

---

## 🏗️ Архитектура

### Структура проекта

```
T1/
├── autopipe/                    # Основной пакет
│   ├── __init__.py
│   ├── __main__.py             # Entry point
│   ├── cli/
│   │   └── main.py             # CLI (Click)
│   ├── core/
│   │   ├── fetcher.py          # Git clone
│   │   ├── stack_detector.py   # Определение стека
│   │   └── pipeline_generator.py
│   ├── detectors/              # Детекторы языков
│   │   ├── base.py
│   │   ├── python_detector.py
│   │   ├── nodejs_detector.py
│   │   ├── java_detector.py
│   │   ├── go_detector.py
│   │   ├── php_detector.py
│   │   └── dotnet_detector.py
│   └── integrations/
│       └── platform_manager.py  # GitLab/SonarQube/Nexus APIs
├── templates/
│   └── gitlab-ci.j2            # Jinja2 шаблон пайплайна
├── docs/
│   └── AUTOPIPE_DOCUMENTATION.md  # Полная документация
├── docker-compose.yml          # Инфраструктура
├── credentials.txt             # Учетные данные
├── pyproject.toml              # Python проект
└── README.md                   # Этот файл
```

### Диаграмма компонентов

```
┌─────────────────────────────────────────────────────────────┐
│                        AutoPipe CLI                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────┐   ┌──────────────┐   ┌───────────────────┐   │
│  │ Fetcher  │──▶│StackDetector │──▶│PipelineGenerator  │   │
│  │(Git Clone)│   │  (6 языков)  │   │  (Jinja2)        │   │
│  └──────────┘   └──────────────┘   └───────────────────┘   │
│                                              │              │
│                                              ▼              │
│                                    ┌───────────────────┐   │
│                                    │ PlatformManager   │   │
│                                    │ - GitLab API      │   │
│                                    │ - SonarQube API   │   │
│                                    │ - Nexus API       │   │
│                                    └───────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### Инфраструктура (Docker Compose)

```
┌─────────────────────────────────────────────────────────────┐
│                 Docker Network (autopipe-net)                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌────────────────┐  ┌────────────────┐  ┌──────────────┐  │
│  │   GitLab CE    │  │   SonarQube    │  │    Nexus     │  │
│  │ :8080 / :5050  │  │     :9000      │  │    :8081     │  │
│  └────────────────┘  └────────────────┘  └──────────────┘  │
│          │                                                  │
│  ┌────────────────┐                                        │
│  │ GitLab Runner  │                                        │
│  │   (Docker)     │                                        │
│  └────────────────┘                                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Pipeline Stages

```
┌──────┐   ┌──────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐
│build │──▶│ test │──▶│ analyze │──▶│ package │──▶│ publish │
└──────┘   └──────┘   └─────────┘   └─────────┘   └─────────┘
                                                       │
              ┌────────────────────────────────────────┘
              ▼
      ┌───────────────┐   ┌──────────────────┐
      │deploy_staging │──▶│deploy_production │
      │    (auto)     │   │    (manual)      │
      └───────────────┘   └──────────────────┘
```

---

## ⚙️ Конфигурация

### Учетные данные по умолчанию

Все учетные данные хранятся в файле `credentials.txt`:

| Сервис | URL | Логин | Пароль/Токен |
|--------|-----|-------|--------------|
| GitLab | http://localhost:8080 | root | см. credentials.txt |
| GitLab API | - | - | glpat-autopipe2025newtoken |
| SonarQube | http://localhost:9000 | admin | Qwerty123 |
| SonarQube API | - | - | squ_40c7ee83cdc698a5ab20d46bdf7d73e12771b414 |
| Nexus | http://localhost:8081 | admin | Qwerty123 |

### Переменные окружения (опционально)

```bash
# GitLab
export GITLAB_URL=http://localhost:8080
export GITLAB_TOKEN=glpat-autopipe2025newtoken

# SonarQube
export SONAR_HOST_URL=http://localhost:9000
export SONAR_TOKEN=squ_40c7ee83cdc698a5ab20d46bdf7d73e12771b414

# Nexus
export NEXUS_URL=http://localhost:8081
export NEXUS_USERNAME=admin
export NEXUS_PASSWORD=Qwerty123
```

---

## 🔧 Устранение неполадок

### GitLab показывает 502 ошибку

```bash
# Перезапуск puma
docker exec t1-gitlab-1 gitlab-ctl restart puma

# Подождите 1-2 минуты
# Проверка статуса
docker exec t1-gitlab-1 gitlab-ctl status

# Если не помогло - проверьте память
docker stats t1-gitlab-1
# GitLab требует минимум 4GB RAM
```

### Pipeline не запускается

```bash
# Проверка раннера
docker exec t1-runner-1 gitlab-runner list

# Проверка логов раннера
docker logs t1-runner-1

# Перерегистрация раннера (если нужно)
docker exec t1-runner-1 gitlab-runner register \
    --non-interactive \
    --url http://gitlab:8080 \
    --registration-token yByzk-TuLoiccqnbREB5 \
    --executor docker \
    --docker-image docker:24-dind \
    --docker-privileged
```

### SonarQube не показывает coverage

1. Проверьте наличие отчета в job logs:
   - `coverage.xml` (Python)
   - `coverage/lcov.info` (Node.js)
   - `target/site/jacoco/jacoco.xml` (Java)

2. Проверьте параметры в analyze job

### Ошибка "401 Unauthorized"

```bash
# Проверьте токен
curl -s --header "PRIVATE-TOKEN: glpat-autopipe2025newtoken" \
    "http://localhost:8080/api/v4/user"
```

### Очистка временных файлов

```bash
# Очистка временных файлов AutoPipe
# Linux/macOS:
rm -rf /tmp/autopipe/*

# Windows (PowerShell):
Remove-Item -Recurse -Force "$env:TEMP\autopipe\*"
```

---

## 📊 Результаты тестирования

| Проект | Язык | Coverage | SonarQube |
|--------|------|----------|-----------|
| gin | Go | **100%** | ✅ Passed |
| httpx | Python | **100%** | ✅ Passed |
| express | Node.js | **98.4%** | ✅ Passed |

---

## 📚 Полезные команды

### Docker Compose

```bash
# Запуск
docker-compose up -d

# Остановка
docker-compose down

# Логи
docker-compose logs -f gitlab
docker-compose logs -f sonarqube

# Перезапуск
docker-compose restart gitlab
```

### GitLab

```bash
# Статус сервисов
docker exec t1-gitlab-1 gitlab-ctl status

# Логи
docker exec t1-gitlab-1 gitlab-ctl tail

# Перезапуск puma
docker exec t1-gitlab-1 gitlab-ctl restart puma
```

### Kubernetes (k3d)

```bash
# Список кластеров
k3d cluster list

# Kubeconfig
k3d kubeconfig get autopipe-cluster > kubeconfig.yaml

# Проверка подов
kubectl get pods -n staging
kubectl get pods -n production
```

---

## 📄 Лицензия

MIT License

## 👥 Авторы

- **Команда 43** - T1 Challenge 2025
- Email: danik160204@gmail.com

## 📎 Ссылки

- [Репозиторий](https://gateway-codemetrics.saas.sferaplatform.ru/app/sourcecode/api/team43/T1.git)
- [Полная документация](docs/AUTOPIPE_DOCUMENTATION.md)
- [GitLab CI Documentation](https://docs.gitlab.com/ee/ci/)
- [SonarQube Documentation](https://docs.sonarqube.org/)
