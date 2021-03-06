add_library(Ctrl
            sdir_ctrl.cpp
            postypes/Robot.h)

if(USE_PYTHON_PLOT)
    add_compile_definitions(PLOT)
    find_package(Python3 COMPONENTS Development NumPy)#Development
    target_include_directories(Ctrl PRIVATE ${Python3_INCLUDE_DIRS} ${Python3_NumPy_INCLUDE_DIRS})
endif()

add_subdirectory(com)
add_dependencies(Ctrl Com)

add_subdirectory(kinematics/direct)
add_subdirectory(kinematics/inverse)
add_dependencies(Ctrl FWKin IVKin)

add_subdirectory(pathplanner/lin)
add_subdirectory(pathplanner/ptp)
add_subdirectory(pathplanner/splines)
add_dependencies(Ctrl PTP LIN SPLINES)

add_subdirectory(postypes)
add_dependencies(Ctrl PosTypes)

if(USE_PYTHON_PLOT)
    target_link_libraries(Ctrl Com PTP LIN SPLINES FWKin IVKin PosTypes Python3::Python Python3::NumPy)
else()
    target_link_libraries(Ctrl Com PTP LIN SPLINES FWKin IVKin PosTypes)
endif()