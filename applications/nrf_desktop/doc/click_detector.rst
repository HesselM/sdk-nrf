.. _nrf_desktop_click_detector:

Click detector module
#####################

The ``click_detector`` module is used to send a ``click_event`` when a known type of click is recorded for the button defined in the module configuration.

Click type
**********

The module records the following click types:

* :cpp:enumerator:`CLICK_SHORT` - Button pressed and released after short time.
* :cpp:enumerator:`CLICK_NONE` - Button pressed and held for a period of time that is too long for :cpp:enumerator:`CLICK_SHORT`, but too short for :cpp:enumerator:`CLICK_LONG`.
* :cpp:enumerator:`CLICK_LONG` - Button pressed and held for a long period of time.
* :cpp:enumerator:`CLICK_DOUBLE` - Two sequences of the button press and release in a short time interval.

The exact values of time intervals for click types are defined in the ``click_detector.c`` file.

Module Events
*************

.. include:: event_propagation.rst
    :start-after: table_click_detector_start
    :end-before: table_click_detector_end

See the :ref:`nrf_desktop_architecture` for more information about the event-based communication in the nRF Desktop application and about how to read this table.

Configuration
*************

Module detects click types based on ``button_event``.
Make sure you define the :ref:`nrf_desktop_buttons` hardware interface.

Set ``CONFIG_DESKTOP_CLICK_DETECTOR_ENABLE`` and define the module configuration.
The configuration (array of :c:type:`struct click_detector_config`) is written in the ``click_detector_def.h`` file located in the board-specific directory in the application configuration folder.

For every click detector, you should define:

* :cpp:member:`key_id` - ID of selected key
* :cpp:member:`consume_button_event` - define if the ``button_event`` with given :cpp:member:`key_id` should be consumed by the module

Implementation details
**********************

Tracing of key states is implemented using periodically submitted work - :c:type:`struct k_delayed_work`.
The work updates the states of traced keys and sends ``click_event``.
The work is not submitted if there is no key for which the state should be updated.
