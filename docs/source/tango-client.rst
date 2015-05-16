Документация к клиентской части TANGO
=====================================

Подключение к томографу

    .. code-block:: python

        tomograph = PyTango.DeviceProxy('188.166.73.250:10000/tomo/tomograph/1')

или прописать в .bashrc (Linux)

    .. code-block:: bash
        
        export TANGO_HOST=188.166.73.250

и подключаться

    .. code-block:: python

        tomograph = PyTango.DeviceProxy('tomo/tomograph/1')


Общие команды
~~~~~~~~~~~~~

.. function:: DevicesInfo()

   TODO

.. function:: SelfTest()

   На данный момент делает ping всех девайсов.


Двигатель
~~~~~~~~~

Атрибуты
--------

.. attribute:: angle_position

   Угод поворота двигателя в градусах.

   :type: float

.. attribute:: horizontal_position

   Положение двигателя по горизонтали.

   :type: int

.. attribute:: vertical_position

   Положение двигателя по вертикали.

   :type: int


Команды
-------

.. function:: MoveAway()

   Убирает объект из поля зрения детектора

.. function:: MoveBack()

   Возвращает объект в поле зрения детектора

.. function:: ResetAnglePosition()

   Делает текущий угол поворота новым нулем.

.. function:: MotorStatus()

   :rtype: str
   :returns: Возвращает JSON-строку следующего формата 

     .. code-block:: javascript

      {
        "state": текущее состояние двигателя: OFF, ON, MOVING (без префикса PyTango)  
        "angle position": угол поврота
        "horizontal position": позиция по горизонтали
        "vertical position": позиция по вертикали
      }


Источник рентгеновского излучения
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Атрибуты
--------

.. attribute:: voltage

   Напряжение в кВ с точностью до десятых. 2,0 кВ <= voltage <= 60,0 кВ

   :type: float


.. attribute:: current

   Ток в мА с точностью до десятых. 2,0 мА <= current <= 80,0 мА

   :type: float


Команды
-------

.. function:: PowerOn()

   Переводит источник рентгеновского излучения в состояние ON

.. function:: PowerOff()

   Переводит источник рентгеновского излучения в состояние OFF

.. function:: XRaySourceStatus()

   :rtype: str
   :returns: Возвращает JSON-строку следующего формата 

     .. code-block:: javascript

      {
        "model": Isovolt 3003
        "state": текущее состояние источника: OFF, ON, STANDBY, FAULT (без префикса PyTango)  
        "voltage": текущее значение напряжения
        "current": текущее значение тока
      }


Заслонка
~~~~~~~~

Команды
-------

.. function:: OpenShutter(time)

   Открывает заслонку на заданное время. Если time == 0, то открывает до вызова :func:`CloseShutter`

   :param time: Время в секундах, через которое нужно закрыть заслонку, или 0, если закрывать не нужно 
   :type time: float

.. function:: CloseShutter(time)

   Закрывает заслонку на заданное время. Если time == 0, то закрывает до вызова :func:`OpenShutter`

   :param time: Время в секундах, через которое нужно открыть заслонку, или 0, если открывать не нужно 
   :type time: float

Точность, с которой можно задавать time неизвестна. Однако, как говорит `StackOverflow <http://stackoverflow.com/questions/1133857/how-accurate-is-pythons-time-sleep>`_, можно рассчитывать на 50 мс.

.. function:: ShutterStatus()

   :rtype: str
   :returns: Возвращает JSON-строку следующего формата 

     .. code-block:: javascript

      {
        "state": текущее состояние двигателя: OPEN, CLOSE (без префикса PyTango)
      }


Детектор
~~~~~~~~

Команды
-------

.. function:: GetFrame(exposure)

   Получает изображение с детектора

   :param exposure: Время экспозиции в 0,1 миллисекунд. 1 <= exposure (0,1 ms) <= 160000, т. е. от 0,1 миллисекунд до 16 секунд.
   :type exposure: int
   :rtype: str
   :returns: Возвращает JSON-строку следующего формата

     .. code-block:: javascript

      {
        "image_data": 
              {
                "image": само изображение
                "exposure": время экспозиции
                "datetime": дата и время получения изображения в формате dd.mm.yyyy hh:mm:ss
                "detector": 
                      {
                        "model": модель детектора
                      }
              }
        "object": 
              {
                "present": True, если объект присутствует, и False иначе
                "angle position": угол поворота объекта
                "horizontal position": положение объекта по горизонтали
                "vertical position": положение объекта по вертикали
              }
        "shutter":
              {
                "open": True, если заслонка открыта, и False иначе
              }

        "X-ray source": 
              {
                "voltage": напряжение
                "current": ток
              }
      }

.. function:: DetectorStatus()

   :rtype: str
   :returns: Возвращает JSON-строку следующего формата 

     .. code-block:: javascript

      {
        "model": Ximea xiRAY
        "state": текущее состояние детектора: OFF, ON, RUNNING (без префикса PyTango)
        "exposure": текущее значение времени экспозиции
      } 


Состояния
---------

PyTango.DevState.OPEN

PyTango.DevState.CLOSE

PyTango.DevState.ON

PyTango.DevState.OFF

и т. д.