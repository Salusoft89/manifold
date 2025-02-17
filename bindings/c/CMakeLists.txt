project(manifoldc)

add_library(${PROJECT_NAME} manifoldc.cpp conv.cpp)

if(MANIFOLD_USE_CUDA)
    set_source_files_properties(manifoldc.cpp conv.cpp PROPERTIES LANGUAGE CUDA)
    set_property(TARGET ${PROJECT_NAME} PROPERTY CUDA_ARCHITECTURES 61)
endif()

if(MANIFOLD_EXPORT)
  target_link_libraries(${PROJECT_NAME} PRIVATE meshIO)
  target_compile_options(${PROJECT_NAME} PUBLIC -DMANIFOLD_EXPORT)
endif()

target_link_libraries(
  ${PROJECT_NAME}
  PRIVATE manifold sdf cross_section
)

target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/include)
target_compile_options(${PROJECT_NAME} PRIVATE ${MANIFOLD_FLAGS})
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)

install(
  TARGETS ${PROJECT_NAME}
  LIBRARY DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}
  COMPONENT bindings
)
install(
  FILES include/manifoldc.h include/types.h
  DESTINATION ${CMAKE_INSTALL_FULL_INCLUDEDIR}/manifold
  COMPONENT devel
)
