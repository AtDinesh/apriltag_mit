

message(STATUS "hdr subfolder, current dir:  " ${CMAKE_CURRENT_BINARY_DIR})

set(header_path "${CMAKE_CURRENT_BINARY_DIR}/apriltag_mit")

# set headers
set(header apriltag_mit.h
${header_path}/DisjointSets.h
${header_path}/Edge.h
${header_path}/FloatImage.h
${header_path}/Apriltags/GrayModel.h
${header_path}/Apriltags/Gridder.h
${header_path}/Apriltags/Line2D.h
${header_path}/Apriltags/LineSegment2D.h
${header_path}/Apriltags/MathUtil.h
${header_path}/Apriltags/Quad.h
${header_path}/Apriltags/Segment.h
${header_path}/Apriltags/Tag16h5.h
${header_path}/Apriltags/Tag25h9.h
${header_path}/Apriltags/Tag36h11.h
${header_path}/Apriltags/tagCodes.h
${header_path}/Apriltags/TagDetection.h
${header_path}/Apriltags/TagDetector.h
${header_path}/Apriltags/TagFamily.h
${header_path}/Apriltags/timer.h
)