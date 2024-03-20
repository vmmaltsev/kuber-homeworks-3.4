# Домашнее задание к занятию «Обновление приложений» - `Мальцев Виктор`

---

Задание 1. Выбрать стратегию обновления приложения и описать ваш выбор

    1. Имеется приложение, состоящее из нескольких реплик, которое требуется обновить.
    2. Ресурсы, выделенные для приложения, ограничены, и нет возможности их увеличить.
    3. Запас по ресурсам в менее загруженный момент времени составляет 20%.
    4. Обновление мажорное, новые версии приложения не умеют работать со старыми.
    5. Вам нужно объяснить свой выбор стратегии обновления приложения.

Ответ:

Учитывая, что ресурсы ограничены и новые версии приложения не совместимы со старыми, я бы предложил использовать стратегию обновления с использованием Blue-Green Deployment или Canary Deployment в контексте Kubernetes (K8s). Обе стратегии имеют свои преимущества и недостатки, и выбор зависит от конкретных требований к приложению и доступных ресурсов.

Blue-Green Deployment
    Blue-Green Deployment подразумевает наличие двух идентичных сред: "синей" (текущей версии приложения) и "зеленой" (новой версии приложения). После того как "зеленая" среда полностью настроена и готова к работе, трафик переключается с "синей" на "зеленую". Это позволяет быстро откатить изменения, если в новой версии обнаружатся проблемы, просто переключившись обратно на "синюю" среду.

Преимущества:

    1. Быстрое переключение между версиями;
    2. Возможность быстрого отката.

Недостатки:

    1. Требует удвоения ресурсов на время развертывания, что может быть проблемой при ограниченных ресурсах.

Canary Deployment
    Canary Deployment подразумевает постепенное внедрение новой версии приложения, когда сначала обновляется небольшая часть реплик, и если все проходит успешно, то постепенно обновляются остальные. Это позволяет минимизировать риски, связанные с выкатыванием новой версии, поскольку в случае возникновения проблем можно быстро отменить обновление.

Преимущества:

    1. Постепенное внедрение снижает риски;
    2. Требует меньше дополнительных ресурсов по сравнению с Blue-Green Deployment.

Недостатки:

    1. Более сложный процесс отката;
    2. Требует более тщательного мониторинга в процессе выкатывания.

Учитывая ограниченность ресурсов я бы рекомендовал использовать Canary Deployment, поскольку эта стратегия позволяет минимизировать необходимость в дополнительных ресурсах и снижает риски, связанные с внедрением новой версии. 
Можно начать с небольшого процента реплик, постепенно увеличивая его и внимательно мониторя работу приложения на каждом этапе. Это позволит гарантировать стабильность работы приложения и эффективно управлять ограниченными ресурсами.

---

Задание 2. Обновить приложение

    1. Создать deployment приложения с контейнерами nginx и multitool. Версию nginx взять 1.19. Количество реплик — 5.
    2. Обновить версию nginx в приложении до версии 1.20, сократив время обновления до минимума. Приложение должно быть доступно.
    3. Попытаться обновить nginx до версии 1.28, приложение должно оставаться доступным.
    4. Откатиться после неудачного обновления.

Ответ:

1. Создаем deployment приложения с контейнерами nginx и multitool.

Файл deployment доступен по ссылке: https://github.com/vmmaltsev/kuber-homeworks-3.4/tree/main/test1

![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_159.png)

2. Обновляем версию nginx в приложении до версии 1.20 с помощью команды:

    microk8s kubectl set image deployment/nginx-multitool nginx=nginx:1.20 --record

    ![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_160.png)

    ![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_161.png)

3. Обновляем версию nginx в приложении до версии 1.28 с помощью команды:

    microk8s kubectl set image deployment/nginx-multitool nginx=nginx:1.28 --record

    ![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_162.png)

    ![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_163.png)

4. Откатываемся после неудачного обновления с помощью команды:

    microk8s kubectl rollout undo deployment/nginx-multitool

    ![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_164.png)

    ![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_165.png)


---

Задание 3*. Создать Canary deployment

    1. Создать два deployment'а приложения nginx.
    2. При помощи разных ConfigMap сделать две версии приложения — веб-страницы.
    3. С помощью ingress создать канареечный деплоймент, чтобы можно было часть трафика перебросить на разные версии приложения.

Ответ:

Деплойменты доступны по ссылке: https://github.com/vmmaltsev/kuber-homeworks-3.4/tree/main/test2

![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_166.png)

![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_167.png)

![alt text](https://github.com/vmmaltsev/screenshot/blob/main/Screenshot_168.png)