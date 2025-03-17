set(SUBSYS_NAME kdtree)
set(SUBSYS_DESC "Point cloud kd-tree library")
set(SUBSYS_DEPS common)

PCL_SUBSYS_OPTION(build "${SUBSYS_NAME}" "${SUBSYS_DESC}" ON)
PCL_SUBSYS_DEPEND(build NAME ${SUBSYS_NAME} DEPS ${SUBSYS_DEPS} EXT_DEPS flann)

PCL_ADD_DOC("${SUBSYS_NAME}")

if(NOT build)
  return()
endif()

set(srcs
  src/kdtree_flann.cpp
)

set(incs
  "include/pcl/${SUBSYS_NAME}/kdtree.h"
  "include/pcl/${SUBSYS_NAME}/io.h"
  "include/pcl/${SUBSYS_NAME}/kdtree_flann.h"
)

set(impl_incs
  "include/pcl/${SUBSYS_NAME}/impl/io.hpp"
  "include/pcl/${SUBSYS_NAME}/impl/kdtree_flann.hpp"
)

find_package(flann PATHS "D:/SoftWare/Library/flann/install/lib/cmake/flann")
find_package(lz4 PATHS "D:/SoftWare/Library/lz4/install/lz4_win64_v1_10_0/lib/cmake/lz4")

set(LIB_NAME "pcl_${SUBSYS_NAME}")
PCL_ADD_LIBRARY(${LIB_NAME} COMPONENT ${SUBSYS_NAME} SOURCES ${srcs} ${incs} ${impl_incs})
target_link_libraries("${LIB_NAME}" pcl_common flann::flann lz4::lz4)

set(EXT_DEPS flann)
PCL_MAKE_PKGCONFIG(${LIB_NAME} COMPONENT ${SUBSYS_NAME} DESC ${SUBSYS_DESC} PCL_DEPS ${SUBSYS_DEPS} EXT_DEPS ${EXT_DEPS})

# Install include files
PCL_ADD_INCLUDES("${SUBSYS_NAME}" "${SUBSYS_NAME}" ${incs})
PCL_ADD_INCLUDES("${SUBSYS_NAME}" "${SUBSYS_NAME}/impl" ${impl_incs})
