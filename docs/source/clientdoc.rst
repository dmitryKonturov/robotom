Документация к клиентской части TANGO
=====================================

Подключение к томографу
	.. code-block:: python
		
		tomograph = PyTango.DeviceProxy('178.62.211.84:10000/tomo/tomograph/1')

	или прописать в .bashrc
	
	.. code-block:: bash
	
		export TANGO_HOST=178.62.211.84:10000
	
	и подключаться
	
	.. code-block:: python
	
		tomograph = PyTango.DeviceProxy('tomo/tomograph/1')

Общие команды
~~~~~~~~~~~~~

DevicesInfo

SelfTest

MotorStatus

.. function:: CurrentPosition()

   Возвращает текущее положение двигателя: позицию по горизонтали, по вертикали и угол поворота.
   На данный момент, используется только угол поворота. Остальные значения не важны.
     
   :rtype: список из трех чисел типа float

.. function:: GotoPosition(new_position)

   Новое положение двигателя 

   :param new_position: Новое 
   :type new_position: list или tuple

ResetCurrentPosition

XRaySourceStatus

PowerOn

PowerOff

SetOperatingMode

ShutterStatus

OpenShutter

CloseShutter

DetectorStatus

GetFrame