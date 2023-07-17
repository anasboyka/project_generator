# {{name}}

A new Flutter project.

## Getting Started

** Due to some package not yet update to null safety, may need to add this
--no-sound-null-safety

## Android Build:

 run dev
flutter run -t lib/main.dart --dart-define=DEFINE_APP_DISPLAY_NAME="[DEV]{{app_name}}" --dart-define=DEFINE_APP_ID=com.{{name}}.dtm --dart-define=DEFINE_ENV=dev

 run prod
flutter run -t lib/main.dart --dart-define=DEFINE_APP_DISPLAY_NAME="{{app_name}}" --dart-define=DEFINE_APP_ID=com.dtm.{{name}} --dart-define=DEFINE_ENV=prod

## iOS Build:
```
// run dev
flutter run -t lib/main.dart --dart-define=DEFINE_APP_DISPLAY_NAME="[DEV]{{app_name}}" --dart-define=DEFINE_APP_ID=com.{{name}}.dtm --dart-define=DEFINE_ENV=dev --dart-define=DEFINE_DEVELOPMENT_TEAM=AS47L5ZRAJ

// run prod
flutter run -t lib/main.dart --dart-define=DEFINE_APP_DISPLAY_NAME="{{app_name}}" --dart-define=DEFINE_APP_ID=com.dtm.{{name}} --dart-define=DEFINE_ENV=prod --dart-define=DEFINE_DEVELOPMENT_TEAM=AS47L5ZRAJ

## Configurations
### Using dart define for iOS
1. Create AppDefine.xcconfig file under ios/Flutter.
2. Create AppDefine-defaults.xcconfig file under ios/Flutter.

``````
DEFINE_APP_DISPLAY_NAME=
DEFINE_APP_ID=
DEFINE_ENV=
DEFINE_DEVELOPMENT_TEAM=
``````

3. Include AppDefine.xcconfig and AppDefine-defaults.xcconfig in Debug.xcconfig and Release.xcconfig.
4. Change CFBundleName value to DEFINE_APP_DISPLAY_NAME in Info.plist.
5. Change DEVELOPMENT_TEAM value to DEFINE_DEVELOPMENT_TEAM, PRODUCT_BUNDLE_IDENTIFIER to DEFINE_APP_ID in project.pbxproj. (debug, profile, release)
6. Add pre action in scheme. (build, run) *Choose Provide build settings from Runner*

add script in Runner.xcodeproj/xcshareddata/runner.xcscheme

- under 

<BuildAction
	parallelizeBuildables = "YES"
	buildImplicitDependencies = "Yes">


<!-- here -->
<PreActions>
         <ExecutionAction
            ActionType = "Xcode.IDEStandardExecutionActionsCore.ExecutionActionType.ShellScriptAction">
            <ActionContent
               title = "Run Script"
               scriptText = "# Type a script or drag a script file from your workspace to insert its path.&#10;&#10;function entry_decode() { echo &quot;${*}&quot; | base64 --decode; }&#10;&#10;IFS=&apos;,&apos; read -r -a define_items &lt;&lt;&lt; &quot;$DART_DEFINES&quot;&#10;&#10;&#10;for index in &quot;${!define_items[@]}&quot;&#10;do&#10;    define_items[$index]=$(entry_decode &quot;${define_items[$index]}&quot;);&#10;done&#10;&#10;printf &quot;%s\n&quot; &quot;${define_items[@]}&quot;|grep &apos;^DEFINE_&apos; &gt; ${SRCROOT}/Flutter/AppDefine.xcconfig&#10;">
               <EnvironmentBuildable>
                  <BuildableReference
                     BuildableIdentifier = "primary"
                     BlueprintIdentifier = "97C146ED1CF9000F007C117D"
                     BuildableName = "Runner.app"
                     BlueprintName = "Runner"
                     ReferencedContainer = "container:Runner.xcodeproj">
                  </BuildableReference>
               </EnvironmentBuildable>
            </ActionContent>
         </ExecutionAction>
</PreActions>
<!-- end -->

-under 

<LaunchAction
      buildConfiguration = "Debug"
      selectedDebuggerIdentifier = "Xcode.DebuggerFoundation.Debugger.LLDB"
      selectedLauncherIdentifier = "Xcode.DebuggerFoundation.Launcher.LLDB"
      launchStyle = "0"
      useCustomWorkingDirectory = "NO"
      ignoresPersistentStateOnLaunch = "NO"
      debugDocumentVersioning = "YES"
      debugServiceExtension = "internal"
      allowLocationSimulation = "YES">


<!-- here -->
	  <PreActions>
         <ExecutionAction
            ActionType = "Xcode.IDEStandardExecutionActionsCore.ExecutionActionType.ShellScriptAction">
            <ActionContent
               title = "Run Script"
               scriptText = "# Type a script or drag a script file from your workspace to insert its path.&#10;&#10;function entry_decode() { echo &quot;${*}&quot; | base64 --decode; }&#10;&#10;IFS=&apos;,&apos; read -r -a define_items &lt;&lt;&lt; &quot;$DART_DEFINES&quot;&#10;&#10;&#10;for index in &quot;${!define_items[@]}&quot;&#10;do&#10;    define_items[$index]=$(entry_decode &quot;${define_items[$index]}&quot;);&#10;done&#10;&#10;printf &quot;%s\n&quot; &quot;${define_items[@]}&quot;|grep &apos;^DEFINE_&apos; &gt; ${SRCROOT}/Flutter/AppDefine.xcconfig&#10;">
               <EnvironmentBuildable>
                  <BuildableReference
                     BuildableIdentifier = "primary"
                     BlueprintIdentifier = "97C146ED1CF9000F007C117D"
                     BuildableName = "Runner.app"
                     BlueprintName = "Runner"
                     ReferencedContainer = "container:Runner.xcodeproj">
                  </BuildableReference>
               </EnvironmentBuildable>
            </ActionContent>
         </ExecutionAction>
      </PreActions>
<!-- end -->



``````
# Type a script or drag a script file from your workspace to insert its path.

function entry_decode() { echo "${*}" | base64 --decode; }

IFS=',' read -r -a define_items <<< "$DART_DEFINES"


for index in "${!define_items[@]}"
do
    define_items[$index]=$(entry_decode "${define_items[$index]}");
done

printf "%s\n" "${define_items[@]}"|grep '^DEFINE_' > ${SRCROOT}/Flutter/AppDefine.xcconfig
``````
