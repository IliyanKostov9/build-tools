cmake_minimum_required(VERSION 3.0)

project(PythonHelloWorld)

find_package(Python3 3.11 REQUIRED)

set(PYTHON_EXECUTABLE ${Python3_EXECUTABLE})
set(VENV_PATH ${CMAKE_SOURCE_DIR}/.venv)
set(PIP "${VENV_PATH}/bin/pip")

add_custom_target(create-venv ALL
    COMMAND ${PYTHON_EXECUTABLE} -m venv ${VENV_PATH}
    COMMENT "Creating Python virtual environment"
)

add_custom_target(install-deps
    COMMAND ${PIP} install -r ${CMAKE_SOURCE_DIR}/requirements.txt --require-virtualenv --disable-pip-version-check --no-cache-dir --no-color
    DEPENDS create-venv
    COMMENT "Installing Python dependencies"
)

add_custom_target(run
    COMMAND ${PYTHON_EXECUTABLE} ${CMAKE_SOURCE_DIR}/src/hello_world.py
    DEPENDS install-deps
    COMMENT "Running Python Hello World script"
)
# depends on
add_dependencies(run install-deps)
