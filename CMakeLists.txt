cmake_minimum_required(VERSION 3.22)

# Define the project name and version
project(AUDIO_PLUGIN_EXAMPLE VERSION 0.0.1)

# Add the JUCE submodule (assumes JUCE is in a subdirectory named "JUCE")
message(STATUS "Adding JUCE submodule")
add_subdirectory(JUCE)
message(STATUS "Adding JUCE submodule - done")

# Define options
option(TRACY_ENABLE "Enable Tracy profiler" ON)

# Plugin format selection: e.g., VST3, Standalone, AU, etc.
set(PLUGIN_FORMAT "VST3" CACHE STRING "Plugin format to build (e.g. VST3, Standalone, AU, etc.)")
string(TOUPPER "${PLUGIN_FORMAT}" PLUGIN_FORMAT_UPPER)  # Convert to uppercase for macro use

# Conditionally add the Tracy profiler submodule if enabled
if(TRACY_ENABLE)
    # Apply Tracy-related options only for VST3 builds
    if(PLUGIN_FORMAT_UPPER STREQUAL "VST3")
        message(STATUS "VST3 build detected — applying Tracy options")
        option(TRACY_ON_DEMAND "Enable on-demand connection for Tracy" ON)
        option(TRACY_NO_BROADCAST "Disable Tracy auto-broadcasting" ON)
        option(TRACY_MANUAL_LIFETIME "Use manual lifetime zones in Tracy" ON)
        option(TRACY_DELAYED_INIT "Delay Tracy initialization" ON)
    endif()

    message(STATUS "Adding Tracy submodule")
    add_subdirectory(tracy)
    message(STATUS "Adding Tracy submodule - done")
endif()

# Add the JUCE plugin definition
# Use conditional formatting logic for VST3 vs others
if(PLUGIN_FORMAT_UPPER STREQUAL "VST3")
    juce_add_plugin(AudioPluginExample
        COMPANY_NAME "YourCompanyName"
        PRODUCT_NAME "AudioPluginExample"
        PLUGIN_MANUFACTURER_CODE Juce
        COPY_PLUGIN_AFTER_BUILD TRUE
        VST3_AUTO_MANIFEST TRUE
        VST3_COPY_DIR "${CMAKE_CURRENT_SOURCE_DIR}/builds_results/${CMAKE_BUILD_TYPE}"
        PLUGIN_CODE Dem0
        FORMATS VST3)
else()
    juce_add_plugin(AudioPluginExample
        COMPANY_NAME "YourCompanyName"
        PRODUCT_NAME "AudioPluginExample"
        PLUGIN_MANUFACTURER_CODE Juce
        COPY_PLUGIN_AFTER_BUILD TRUE
        PLUGIN_CODE Dem0
        FORMATS ${PLUGIN_FORMAT})
endif()

# Generate the JUCE plugin header
juce_generate_juce_header(AudioPluginExample)

# Add source files to the target
target_sources(AudioPluginExample
    PRIVATE
        src/PluginEditor.cpp
        src/PluginProcessor.cpp)

# Define plugin-wide preprocessor macros
# Disabling optional JUCE features and defining the selected plugin format
target_compile_definitions(AudioPluginExample
    PUBLIC
        JUCE_WEB_BROWSER=0           # Disable JUCE web browser module
        JUCE_USE_CURL=0              # Disable CURL integration
        JUCE_VST3_CAN_REPLACE_VST2=0 # Prevent VST3 from replacing VST2
        PLUGIN_FORMAT_${PLUGIN_FORMAT_UPPER}  # e.g., PLUGIN_FORMAT_VST3
)

# Link JUCE and Tracy libraries to the plugin target
target_link_libraries(AudioPluginExample
    PRIVATE
        juce::juce_audio_utils
        $<$<BOOL:${TRACY_ENABLE}>:Tracy::TracyClient>  # Link Tracy if enabled
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags)
