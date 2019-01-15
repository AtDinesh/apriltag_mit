# set source files
set(source DisjointSets.cc
      Edge.cc
      FloatImage.cc
      GrayModel.cc
      Line2D.cc
      LineSegment2D.cc
      Quad.cc
      Segment.cc
      TagDetection.cc
      TagDetector.cc
      TagFamily.cc
    )

# configure config.hpp.in
# configure_file("config.hpp.in" "${CMAKE_CURRENT_BINARY_DIR}/config_impl.hpp")