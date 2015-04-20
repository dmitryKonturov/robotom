Документация к клиентской части TANGO
=====================================

Подключение к томографу
    .. code-block:: python

        tomograph = PyTango.DeviceProxy('46.101.31.93:10000/tomo/tomograph/1')

или прописать в .bashrc (Linux)
    .. code-block:: bash
        
        export TANGO_HOST=46.101.31.93:10000

и подключаться
    .. code-block:: python

        tomograph = PyTango.DeviceProxy('tomo/tomograph/1')


Общие команды
~~~~~~~~~~~~~

.. function:: DevicesInfo()

   TODO

.. function:: SelfTest()

   TODO


Двигатель
~~~~~~~~~

.. function:: MotorStatus()

   TODO

.. function:: CurrentPosition()

   Возвращает текущее положение двигателя: позицию по горизонтали, по вертикали и угол поворота.
   На данный момент, используется только угол поворота. Остальные значения не важны.

   :rtype: list из трех чисел типа int


.. function:: GotoPosition(new_position)

   Переводит двигатель в новое положение 

   :param new_position: Новое положение двигателя: по горизонтали, по вертикали, угол поворота (0 <= angle <= 3600. Измеряется в 0,1 градуса. Реальное значение может отличаться!)
   :type new_position: list или tuple

.. function:: ResetCurrentPosition()

   Делает текущее положение новым нулем.



Источник рентгеновского излучения
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. function:: XRaySourceStatus()

   TODO

.. function:: PowerOn()

   Переводит источник рентгеновского излучения в состояние OPEN

.. function:: PowerOff()

   Переводит источник рентгеновского излучения в состояние CLOSE

.. function:: SetOperatingMode(new_operating_mode)

   :param new_operating_mode: voltage, current
   :type new_operating_mode: list или tuple 
   :param voltage: Новое значение напряжения в 0,1 кВ. 20 <= voltage <= 600, т. е. 2,0 кВ <= voltage <= 60,0 кВ
   :type voltage: int 
   :param current: Новое значение тока в 0,1 мА. 20 <= current <= 800, т. е. 2,0 мА <= current <= 80,0 мА
   :type current: int
   :raises: 


Заслонка
~~~~~~~~

.. function:: ShutterStatus()

   TODO

.. function:: OpenShutter(time)

   Открывает заслонку на заданное время. Если time == 0, то открывает до вызова :func:`CloseShutter`

   :param time: Время в секундах, через которое нужно закрыть заслонку, или 0, если закрывать не нужно 
   :type time: int 

.. function:: CloseShutter(time)

   Закрывает заслонку на заданное время. Если time == 0, то закрывает до вызова :func:`OpenShutter`

   :param time: Время в секундах, через которое нужно открыть заслонку, или 0, если открывать не нужно 
   :type time: int 

Детектор
~~~~~~~~

.. function:: DetectorStatus()

   TODO

.. function:: GetFrame(exposure)

   Получает изображение с детектора

   :param exposure: Время экспозиции в миллисекундах
   :param type: int
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
              "angle position": угол поворота объекта
            }
      "shutter":
            {
              "open": True, если заслонка открыта и False иначе
            }

      "X-ray source": 
            {
              "voltage": напряжение
              "current": ток
            }
    }


Состояния
---------

PyTango._PyTango.DevState.OPEN

PyTango._PyTango.DevState.CLOSE

PyTango._PyTango.DevState.ON

PyTango._PyTango.DevState.OFF
