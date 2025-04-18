
==========
 Skyfield
==========

.. raw:: html

   <img class="logo" src="_static/logo.png">

.. rst-class:: motto

   *Elegant Astronomy for Python*

Skyfield computes positions for the stars, planets,
and satellites in orbit around the Earth.
Its results should agree
with the positions generated by the United States Naval Observatory
and their *Astronomical Almanac*
to within 0.0005 arcseconds (half a “mas” or milliarcsecond).

* Written in pure Python.
* Installs without any compilation.
* Supports Python 2.7 and Python 3.

Computing the position of Mars in the sky is as easy as:

.. testsetup::

   __import__('skyfield.tests.fixes').tests.fixes.setup()

.. testcode::

    from skyfield.api import load

    # Create a timescale and ask the current time.
    ts = load.timescale()
    t = ts.now()

    # Load the JPL ephemeris DE421 (covers 1900-2050).
    planets = load('de421.bsp')
    earth, mars = planets['earth'], planets['mars']

    # What's the position of Mars, viewed from Earth?
    astrometric = earth.at(t).observe(mars)
    ra, dec, distance = astrometric.radec()

    print(ra)
    print(dec)
    print(distance)

.. testoutput::

    10h 47m 56.24s
    +09deg 03' 23.1"
    2.33251 au

Skyfield can compute geocentric coordinates,
as shown in the example above,
or topocentric coordinates specific to your location
on the Earth’s surface:

.. testcode::

    from skyfield.api import N, W, wgs84

    boston = earth + wgs84.latlon(42.3583 * N, 71.0636 * W)
    astrometric = boston.at(t).observe(mars)
    alt, az, d = astrometric.apparent().altaz()

    print(alt)
    print(az)

.. testoutput::

    25deg 27' 54.0"
    101deg 33' 44.1"

While Skyfield itself has no dependency on the `AstroPy`_ library,
it’s willing to accept AstroPy time objects as input
and return results in native AstroPy units:

.. testcode::

    from astropy import units as u
    xyz = astrometric.xyz.to(u.au)
    altitude = alt.to(u.deg)

    print(xyz)
    print('{0:0.03f}'.format(altitude))

.. testoutput::

    [-2.19049548  0.71236701  0.36712443] AU
    25.465 deg

Academics can cite Skyfield as
`ascl:1907.024 <https://ascl.net/1907.024>`_
or
`2019ascl.soft07024R <https://ui.adsabs.harvard.edu/abs/2019ascl.soft07024R/abstract>`_
:ref:`(more…) <citing-skyfield>`

Documentation
=============

Skyfield’s documentation lives here on the main Skyfield web site:

* :doc:`toc`
* :doc:`installation`
* :doc:`api`
* :ref:`changelog`

But the source code and issue tracker live on other web sites:

* `Packages: at the Python Package Index <https://pypi.python.org/pypi/skyfield>`_
* `Source: on GitHub <https://github.com/skyfielders/python-skyfield/>`_
* `Discussion: on GitHub <https://github.com/skyfielders/python-skyfield/discussions>`_
* `Issues: on GitHub <https://github.com/skyfielders/python-skyfield/issues>`_

See the :ref:`changelog` for the current version’s release notes —
and also for the updates that landed with each previous version!

.. testcleanup::

   __import__('skyfield.tests.fixes').tests.fixes.teardown()

.. _astropy: http://docs.astropy.org/en/stable/
