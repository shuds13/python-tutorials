# Get numpy build configuration.

Get basic configuration including BLAS libraries.

    import numpy
    numpy.__config__.show()

Get full configuration details including compiler options/environment

    import numpy.distutils
    np_config_vars = numpy.distutils.unixccompiler.sysconfig.get_config_vars()
    
    import pprint
    pprint.pprint(np_config_vars)


