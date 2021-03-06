# Copyright (c) 2017, United States Government, as represented by the
# Administrator of the National Aeronautics and Space Administration.
# 
# All rights reserved.
# 
# The Astrobee platform is licensed under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with the
# License. You may obtain a copy of the License at
# 
#     http://www.apache.org/licenses/LICENSE-2.0
# 
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
# License for the specific language governing permissions and limitations
# under the License.

project(pico_driver)

catkin_package(
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS roscpp nodelet ff_msgs
  DEPENDS ${PROJECT_NAME}
)

link_directories(${royale_LIB_DIR})

create_library(TARGET ${PROJECT_NAME}
  LIBS ${catkin_LIBRARIES} ${royale_LIBRARIES} config_reader ff_nodelet
  INC  ${catkin_INCLUDE_DIRS} ${royale_INCLUDE_DIRS}
)

if(CATKIN_ENABLE_TESTING)
  # Pico flexx initialization fault tester
  add_rostest_gtest(test_init_pico_flexx
    test/test_init_pico_flexx.test
    test/test_init_pico_flexx.cc
  )

  target_link_libraries(test_init_pico_flexx
    ${catkin_LIBRARIES} config_reader ff_nodelet
  )
endif()

create_tool_targets(DIR tools
  LIBS ${catkin_LIBRARIES} ${royale_LIBRARIES} uvc
  INC ${catkin_INCLUDE_DIRS} ${royale_INCLUDE_DIRS}
  DEPS ff_msgs opencv
)

install_launch_files()
