Инструкция.
(На основе гайда https://github.com/ultralytics/yolov5/wiki/Train-Custom-Data
- там все довольно лаконично и понятно)

0. Клонирование репозитория и установка всех зависимостей:

$ git clone https://github.com/ultralytics/yolov5  # clone repo
$ cd yolov5
$ pip install -r requirements.txt  # install dependencies

*0.5. (Опционально)
Перед обучением модели для визуализации процесса обучения и результатов онлайн можно установить библиотеку wandb:
$ pip install wandb
Если библиотека установлена, после запуска обучения алгоритм предложит следующее:

wandb: (1) Create a W&B account
wandb: (2) Use an existing W&B account
wandb: (3) Don't visualize my results
←[34m←[1mwandb←[0m: Enter your choice:

Сервис https://www.wandb.com/ каким-то образом позволяет онлайн отслеживать процесс и результат обучения.

Для работы на видеокарте потребуется карта от NVidia и установленная CUDA https://developer.nvidia.com/cuda-downloads?

1. Создание "имя_датасета".yaml.
Файл должен лежать в директории yolov5/data/"имя_датасета".yaml. Наш файл FacadeDatasetConverted.yaml (приложен в архиве) имеет следующий вид:

# train and val data as 1) directory: path/images/, 2) file: path/images.txt, or 
# 3) list: [path1/images/, path2/images/]
train: ../FacadeDatasetConverted/images/train/
val: ../FacadeDatasetConverted/images/test/  
# number of classes
nc: 6
# class names
names: [ 'window', 'cantilever', 'arch', 'signboard', 'win_ad', 'door' ]

2. Создание labels.
Наши лейблы готовы и лежат в архиве.
Файлы с лейблами имеют расширения *.txt, каждый файл назван соответственно названию изображения из датасета. Формат записи лейблов: class x_center y_center width height, каждое значение представлено относительно ширины и высоты всего изображения. Например, 1 вывеска и 2 окна:

3 0.61953125 0.5694444444444444 0.0984375 0.04259259259259259
0 0.23177083333333334 0.07592592592592592 0.084375 0.15185185185185185
0 0.22005208333333334 0.39305555555555555 0.0671875 0.2101851851851852

Если лейблы представлены в формате XML (формат PASCAL Visual Object Classes), существуют конвертеры в формат YOLO, например:
https://gist.github.com/Amir22010/a99f18ca19112bc7db0872a36a03a1ec

3. Организация директорий.
Директории должны быть организованы следующим образом:
--yolov5
	--data
	       --FacadeDatasetConverted.yaml
	--inference
	...
--FacadeDatasetConverted
	--images
		--train
		--test
	--labels
		--train
		--test
		
4. Обучение модели.
Пример команды на обучение:
$ python train.py --img 640 --batch 16 --epochs 5 --data FacadeDatasetConverted.yaml --weights yolov5s.pt

Последним параметром можно выбрать тип модели: yolov5s.pt, yolov5m.pt, yolov5l.pt, yolov5x.pt
Параметр --batch устанавливает количество изображений в одной пачке для обработки (чем больше batch, тем больше требуется памяти. При нехватке памяти выдает ошибку в начале первой эпохи).
Параметр --epochs устанавливает количество эпох (видимо, разные итерации обучения / варианты обучения).

5. Детекция объектов на основе обученной модели.
Пример команады на детекцию объектов на изображениях:

$ python detect.py --source data/images --weights runs/train/exp1/weights/last.pt

--weights runs/train/exp1/weights/last.pt - параметр, который указывает обученную модель
