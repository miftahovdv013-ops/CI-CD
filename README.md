# Домашняя работа к занятию "CI/CD"

## Цель задания
Научиться пользоваться инструментом CI/CD GitlabCk.

## Выполненные требования:

1. **Теги в тасках:** `netology`
2. **Шаги пайплайна:** `build`, `test`
3. **Build этап:**
   - Вывод "начало Building"
   - Создание папки `build`
   - Создание файла `info.txt` в папке `build`
4. **Test этап:**
   - Вывод "Testing"
   - Проверка наличия файла `info.txt` в папке `build`

## Конфигурация CI/CD

### Файл .gitlab-ci.yml
```yaml
stages:
  - build
  - test

variables:
  BUILD_DIR: "build"

build-job:
  stage: build
  tags:
    - netology
  script:
    - echo "начало Building"
    - mkdir -p $BUILD_DIR
    - echo "File created by GitLab CI/CD" > $BUILD_DIR/info.txt
    - echo "Build completed successfully"
  artifacts:
    paths:
      - $BUILD_DIR/
    expire_in: 1 hour

test-job:
  stage: test
  tags:
    - netology
  script:
    - echo "Testing"
    - if [ -f "$BUILD_DIR/info.txt" ]; then
        echo "File info.txt exists!";
        cat $BUILD_DIR/info.txt;
      else
        echo "File info.txt not found!";
        exit 1;
      fi
    - echo "Test completed successfully"
  needs:
    - build-job
