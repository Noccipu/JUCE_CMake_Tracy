cmake_minimum_required(VERSION 3.22)

# Define the project name and version
project(AUDIO_PLUGIN_EXAMPLE VERSION 0.0.1)

# Add the JUCE submodule (assumes JUCE is in a subdirectory named JUCE)
message(STATUS "Adding JUCE submodule")
add_subdirectory(JUCE)
message(STATUS "Adding JUCE submodule - done")

# Define options related to Tracy instrumentation
option(TRACY_ENABLE "Enable Tracy profiler" ON)
option(TRACY_ON_DEMAND "Enable on-demand connection for Tracy" ON)
option(TRACY_NO_BROADCAST "Disable Tracy auto-broadcasting" ON)
option(TRACY_MANUAL_LIFETIME "Use manual lifetime zones in Tracy" ON)
option(TRACY_DELAYED_INIT "Delay Tracy initialization" ON)

# Conditionally add the Tracy submodule only if TRACY_ENABLE is ON
if(TRACY_ENABLE)
    message(STATUS "Adding Tracy submodule")
    add_subdirectory(tracy)
    message(STATUS "Adding Tracy submodule - done")
endif()

# Define the plugin using the JUCE plugin API
juce_add_plugin(AudioPluginExample
    COMPANY_NAME "YourCompanyName"
    PRODUCT_NAME "AudioPluginExample"
    PLUGIN_MANUFACTURER_CODE Juce
    COPY_PLUGIN_AFTER_BUILD TRUE
    VST3_AUTO_MANIFEST TRUE
    VST3_COPY_DIR "${CMAKE_CURRENT_SOURCE_DIR}/builds_results/${CMAKE_BUILD_TYPE}"
    PLUGIN_CODE Dem0
    FORMATS Standalone VST3)

# Generate the JUCE header file
juce_generate_juce_header(AudioPluginExample)

# Add source files to the target
target_sources(AudioPluginExample
    PRIVATE
        src/PluginEditor.cpp
        src/PluginProcessor.cpp)

# Define compile-time flags for JUCE
target_compile_definitions(AudioPluginExample
    PUBLIC
        JUCE_WEB_BROWSER=0       # Disable web browser component
        JUCE_USE_CURL=0          # Disable CURL support
        JUCE_VST3_CAN_REPLACE_VST2=0)  # Prevent VST3 from replacing VST2

# Link libraries to the plugin
target_link_libraries(AudioPluginExample
    PRIVATE
        # AudioPluginData           # Link this if a binary data target is created
        juce::juce_audio_utils

        # Link Tracy only if it's enabled
        $<$<BOOL:${TRACY_ENABLE}>:Tracy::TracyClient>
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags)
