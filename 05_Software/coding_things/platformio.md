a good way tp develop for embeded boards and not being bound to the ardunio IDE

# installation under linux

simpley execute this to install 
```shell
curl -fsSL -o get-platformio.py https://raw.githubusercontent.com/platformio/platformio-core-installer/master/get-platformio.py
python3 get-platformio.py
```
## emacs installation

adding the following line to the your emacs config
```lisp
;; PlatformIO-mode for embedded things
(use-package platformio-mode
	:ensure t)
```

# creating tests for embeded hardware
[PlatformIo Documentation](https://docs.platformio.org/en/latest/advanced/unit-testing/index.html)

The supported frameworks are as follows as Taken from [PlatformIO Docs](https://docs.platformio.org/en/latest/advanced/unit-testing/frameworks/index.html)


| Framework                                                                                                                               | [Test Types](https://docs.platformio.org/en/latest/advanced/unit-testing/runner.html#unit-testing-runner-test-types) | [Development Platforms](https://docs.platformio.org/en/latest/platforms/index.html#platforms)                                                                                                                                                                                                             | Mocking |
| --------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| [Doctest](https://docs.platformio.org/en/latest/advanced/unit-testing/frameworks/doctest.html#unit-testing-frameworks-doctest)          | Native                                                                                                               | [Native](https://docs.platformio.org/en/latest/platforms/native.html#platform-native)                                                                                                                                                                                                                     | No      |
| [GoogleTest](https://docs.platformio.org/en/latest/advanced/unit-testing/frameworks/googletest.html#unit-testing-frameworks-googletest) | Any                                                                                                                  | [Native](https://docs.platformio.org/en/latest/platforms/native.html#platform-native), [Espressif 8266](https://docs.platformio.org/en/latest/platforms/espressif8266.html#platform-espressif8266), [Espressif 32](https://docs.platformio.org/en/latest/platforms/espressif32.html#platform-espressif32) | Yes     |
| [Unity](https://docs.platformio.org/en/latest/advanced/unit-testing/frameworks/unity.html#unit-testing-frameworks-unity)                | Any                                                                                                                  | Any                                                                                                                                                                                                                                                                                                       | No      |

### hiding not test related source code 

> Please note that you will need to use `#ifndef PIO_UNIT_TESTING` and `#endif` guard to hide non-test related source code. For example, own `main()`, `setup() / loop()`, or `app_main()` functions.

as taken directly from the [PlatformIO Docs](https://docs.platformio.org/en/latest/advanced/unit-testing/structure/shared-code.html)


### Using Unity

[PlatformIO docs on Unity](https://docs.platformio.org/en/latest/advanced/unit-testing/frameworks/unity.html)
