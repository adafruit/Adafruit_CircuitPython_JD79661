Introduction
============


.. image:: https://readthedocs.org/projects/adafruit-circuitpython-jd79661/badge/?version=latest
    :target: https://docs.circuitpython.org/projects/jd79661/en/latest/
    :alt: Documentation Status


.. image:: https://raw.githubusercontent.com/adafruit/Adafruit_CircuitPython_Bundle/main/badges/adafruit_discord.svg
    :target: https://adafru.it/discord
    :alt: Discord


.. image:: https://github.com/adafruit/Adafruit_CircuitPython_JD79661/workflows/Build%20CI/badge.svg
    :target: https://github.com/adafruit/Adafruit_CircuitPython_JD79661/actions
    :alt: Build Status


.. image:: https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json
    :target: https://github.com/astral-sh/ruff
    :alt: Code Style: Ruff

CircuitPython library for the JD79661 e-paper driver IC


Dependencies
=============
This driver depends on:

* `Adafruit CircuitPython <https://github.com/adafruit/circuitpython>`_

Please ensure all dependencies are available on the CircuitPython filesystem.
This is easily achieved by downloading
`the Adafruit library and driver bundle <https://circuitpython.org/libraries>`_
or individual libraries can be installed using
`circup <https://github.com/adafruit/circup>`_.

`Purchase one from the Adafruit shop <http://www.adafruit.com/products/6373>`_

Installing from PyPI
=====================

On supported GNU/Linux systems like the Raspberry Pi, you can install the driver locally `from
PyPI <https://pypi.org/project/adafruit-circuitpython-jd79661/>`_.
To install for current user:

.. code-block:: shell

    pip3 install adafruit-circuitpython-jd79661

To install system-wide (this may be required in some cases):

.. code-block:: shell

    sudo pip3 install adafruit-circuitpython-jd79661

To install in a virtual environment in your current project:

.. code-block:: shell

    mkdir project-name && cd project-name
    python3 -m venv .venv
    source .env/bin/activate
    pip3 install adafruit-circuitpython-jd79661

Installing to a Connected CircuitPython Device with Circup
==========================================================

Make sure that you have ``circup`` installed in your Python environment.
Install it with the following command if necessary:

.. code-block:: shell

    pip3 install circup

With ``circup`` installed and your CircuitPython device connected use the
following command to install:

.. code-block:: shell

    circup install adafruit_jd79661

Or the following command to update an existing version:

.. code-block:: shell

    circup update

Usage Example
=============

.. code-block:: python

    import time

    import board
    import displayio
    from fourwire import FourWire

    import adafruit_jd79661

    displayio.release_displays()

    spi = board.SPI()
    epd_cs = board.D9
    epd_dc = board.D10
    epd_reset = board.D6
    epd_busy = board.D5

    display_bus = FourWire(spi, command=epd_dc, chip_select=epd_cs, reset=epd_reset, baudrate=1000000)
    time.sleep(1)

    display = adafruit_jd79661.JD79661(
        display_bus,
        width=250,
        height=122,
        busy_pin=epd_busy,
        rotation=270,
        colstart=0,
        highlight_color=0x00FF00,
        highlight_color2=0xFF0000,
    )

    g = displayio.Group()

    pic = displayio.OnDiskBitmap("/display-ruler-640x360.bmp")
    t = displayio.TileGrid(pic, pixel_shader=pic.pixel_shader)
    g.append(t)

    display.root_group = g

    display.refresh()

    print("refreshed")

    time.sleep(display.time_to_refresh + 5)
    # Always refresh a little longer. It's not a problem to refresh
    # a few seconds more, but it's terrible to refresh too early
    # (the display will throw an exception when if the refresh
    # is too soon)
    print("waited correct time")


    # Keep the display the same
    while True:
        time.sleep(10)


Documentation
=============
API documentation for this library can be found on `Read the Docs <https://docs.circuitpython.org/projects/jd79661/en/latest/>`_.

For information on building library documentation, please check out
`this guide <https://learn.adafruit.com/creating-and-sharing-a-circuitpython-library/sharing-our-docs-on-readthedocs#sphinx-5-1>`_.

Contributing
============

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/adafruit/Adafruit_CircuitPython_JD79661/blob/HEAD/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.
