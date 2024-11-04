# Deploy React, Spring Boot & PostgreSQL Fullstack application on Kubernetes

### Описание
Этот учебный проект демонстрирует создание, контейнеризацию и оркестрацию веб-приложения с фронтендом на **React**, бэкендом на **Spring Boot** и базой данных **PostgreSQL**. Проект использует **Helm** для управления развертыванием в **Kubernetes** и включает **Prometheus** и **Grafana** для мониторинга и визуализации метрик. В качестве CI/CD решений используются **GitHub Actions** и **GitLab CI**.

### Основные технологии
- **React** – Фронтенд-приложение
- **Spring Boot** – Бэкенд-приложение
- **PostgreSQL** – База данных
- **Docker** – Контейнеризация компонентов
- **Docker Compose** – Локальное развертывание в контейнерах
- **Kubernetes** – Оркестрация контейнеров в кластере
- **Helm** – управление Kubernetes-развертыванием
- **Prometheus** – Система мониторинга
- **Grafana** – Инструмент визуализации метрик
- **GitHub Actions** и **GitLab CI** – автоматизация CI/CD

### Структура проекта
- **client/** – React-приложение (фронтенд)
- **server/** – Spring Boot-приложение (бэкенд)
- **kubernetes/** – Манифесты Kubernetes
- **docker-compose.yml** – Конфигурация для Docker Compose
- **Dockerfile** – Dockerfile для контейнеризации приложений


### Возможности
- Автоматизированное развертывание с использованием CI/CD
- Управление Kubernetes-развертыванием с помощью Helm
- Сбор и визуализация метрик с помощью Prometheus и Grafana
- Простое локальное развертывание с Docker Compose


### Установка и запуск

#### Требования
- **Docker** и **Docker Compose** для локального развертывания
- **Kubernetes** (можно использовать Minikube для локального кластера)
- **kubectl** для управления Kubernetes-кластером
- **Helm**
- **Prometheus** и **Grafana** для мониторинга

#### Запуск с Docker Compose
1. Клонируйте репозиторий:
   ```bash
   git clone https://github.com/Meruyert98/examDevops.git
   cd examDevops
   ```
2. Создайте файл .env с параметрами подключения к базе данных и другими переменными окружения (см. пример ниже).
3. Запустите приложение с помощью Docker Compose:
      ```bash
      docker-compose up --build
      ```

#### Запуск в Kubernetes
1. Убедитесь, что Kubernetes-кластер активен (для локального запуска можно использовать Minikube).
2. Примените манифесты Kubernetes для развертывания приложения и сервисов:
      ```bash
      kubectl apply -f k8s/
      ```
3. Настройте Prometheus и Grafana для сбора метрик и визуализации.

#### Установка и развертывание с Helm и Kubernetes
- Убедитесь, что Kubernetes-кластер запущен, и установлен Helm.
- Перейдите в папку helm и установите чарт:
```bash
helm install my-app ./helm/project-chart
```
Это установит приложение и все необходимые зависимости в Kubernetes-кластере.

#### Описание Helm-чарта
Чарт Helm (helm/project-chart/) настроен для развертывания всех компонентов приложения:
- values.yaml: Содержит переменные для настройки базы данных, API, Prometheus и Grafana.
- templates/: Манифесты Kubernetes для React, Spring Boot, PostgreSQL, а также для конфигурации мониторинга.

#### Пример файла .env
```plaintext
SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/yourdbname
SPRING_DATASOURCE_USERNAME=yourdbuser
SPRING_DATASOURCE_PASSWORD=yourdbpassword
REACT_APP_SERVER_URL=http://localhost:8080
```

### CI/CD с GitHub Actions

#### Файлы конфигурации
- `.github/workflows/build.yml` – Сборка, тестирование и публикация Docker-образов.
- `.github/workflows/deploy.yml` – Развертывание приложения в Kubernetes или другом целевом окружении.

#### Пример файла `.github/workflows/build.yml`

```yaml
name: Build and Test

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up JDK 11
        uses: actions/setup-java@v2
        with:
          java-version: '11'

      - name: Build with Maven
        run: mvn clean install -DskipTests

      - name: Build Docker image
        run: docker build -t username/project-name:latest .

      - name: Push Docker image to registry
        env:
          DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
          DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
        run: |
          echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
          docker push username/project-name:latest
```

#### Настройка CI/CD
Для обоих CI/CD решений вам необходимо:
- Задать секреты (например, DOCKER_USERNAME, DOCKER_PASSWORD, KUBECONFIG и другие) в настройках репозитория на GitHub и GitLab.
- Убедиться, что все зависимости проекта установлены в файле Dockerfile.


#### Мониторинг с Prometheus и Grafana
Для мониторинга приложения:
- Установите Prometheus и Grafana в вашем Kubernetes-кластере или используйте Docker Compose.
- Настройте Prometheus для сбора метрик Spring Boot.
- Подключите Grafana к Prometheus и создайте дашборды для визуализации данных.

