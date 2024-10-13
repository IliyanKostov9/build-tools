cmake_minimum_required(VERSION 3.0)

project(JavaHello NONE)

find_package(Java 21.0 REQUIRED)
include(UseJava)

set(JAVA_EXECUTABLE ${Java_JAVA_EXECUTABLE})
set(CMAKE_JAVA_COMPILE_FLAGS "-source" "8" "-target" "8")
set(JAR_NAME JavaHelloJar)
set(JAVA_SOURCE_DIRECTORY ${CMAKE_SOURCE_DIR}/src/main/java)
set(JAVA_TARGET_DIRECTORY ${CMAKE_SOURCE_DIR}/target)
set(JAVA_SOURCE_FILES ${JAVA_SOURCE_DIRECTORY}/com/mycompany/app/App.java)

set(MAIN_CLASS "com.mycompany.app.App")

message("Jar source directory ${JAVA_SOURCE_DIRECTORY}")
message("Jar file ${JAR_NAME}")

add_jar(${JAR_NAME} ${JAVA_SOURCE_FILES}
				OUTPUT_DIR ${JAVA_TARGET_DIRECTORY}
				OUTPUT_NAME ${JAR_NAME}
				VERSION 1.0.0
				ENTRY_POINT ${MAIN_CLASS}
)

find_jar(JAR_PATH
				NAMES ${JAR_NAME}
				PATHS ${JAVA_TARGET_DIRECTORY}
				VERSIONS 1.0.0
)

add_custom_target(run
				COMMENT "Running compiled program at: ${JAR_PATH}"
				COMMAND ${JAVA_EXECUTABLE} -jar ${JAR_PATH}
)
