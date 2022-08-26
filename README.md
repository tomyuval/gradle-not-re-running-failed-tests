# A minimal example of Gradle not re-running failed tests (on a multi-platform Kotlin library)

When running `./gradlew check`, the tests fail ([as they're supposed to](src/commonTest/kotlin/T.kt)):
```
$ ./gradlew check

> Configure project :
Kotlin Multiplatform Projects are an Alpha feature. See: https://kotlinlang.org/docs/reference/evolution/components-stability.html. To hide this message, add 'kotlin.mpp.stability.nowarn=true' to the Gradle properties.

The property 'kotlin.mpp.enableGranularSourceSetsMetadata=true' has no effect in this and future Kotlin versions, as Hierarchical Structures support is now enabled by default. It is safe to remove the property.

The property 'kotlin.native.enableDependencyPropagation=false' has no effect in this and future Kotlin versions, as Kotlin/Native dependency commonization is now enabled by default. It is safe to remove the property.


> Task :kotlinNpmInstall
warning Ignored scripts due to flag.

> Task :jsLegacyBrowserTest

T.t FAILED
    AssertionError at /var/folders/5r/jzhxz6rd7fz236lzbs9sm7m00000gn/T/_karma_webpack_5638/commons.js:84

1 test completed, 1 failed
There were failing tests

> Task :jsIrBrowserTest

T.t FAILED
    AssertionError at /var/folders/5r/jzhxz6rd7fz236lzbs9sm7m00000gn/T/_karma_webpack_828040/commons.js:17012

1 test completed, 1 failed
There were failing tests

> Task :jvmTest

T[jvm] > t()[jvm] FAILED
    org.opentest4j.AssertionFailedError at T.kt:5

1 test completed, 1 failed
There were failing tests

> Task :nativeTest

T.t FAILED
    kotlin.AssertionError at /opt/buildAgent/work/67fbc2b507315583/kotlin/kotlin-native/runtime/src/main/kotlin/kotlin/Throwable.kt:24

1 test completed, 1 failed
There were failing tests

> Task :allTests FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':allTests'.
> There were failing tests. See the report at: file:///path/to/project/build/reports/tests/allTests/index.html

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.

* Get more help at https://help.gradle.org

Deprecated Gradle features were used in this build, making it incompatible with Gradle 8.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

See https://docs.gradle.org/7.4.2/userguide/command_line_interface.html#sec:command_line_warnings

BUILD FAILED in 15s
29 actionable tasks: 26 executed, 3 up-to-date
```
(exit code: 1).

But when running `./gradlew check` a second time, everything is "up-to-date", so Gradle doesn't do anything – and does
not fail:
```
$ ./gradlew check

> Configure project :
Kotlin Multiplatform Projects are an Alpha feature. See: https://kotlinlang.org/docs/reference/evolution/components-stability.html. To hide this message, add 'kotlin.mpp.stability.nowarn=true' to the Gradle properties.

The property 'kotlin.mpp.enableGranularSourceSetsMetadata=true' has no effect in this and future Kotlin versions, as Hierarchical Structures support is now enabled by default. It is safe to remove the property.

The property 'kotlin.native.enableDependencyPropagation=false' has no effect in this and future Kotlin versions, as Kotlin/Native dependency commonization is now enabled by default. It is safe to remove the property.


Deprecated Gradle features were used in this build, making it incompatible with Gradle 8.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

See https://docs.gradle.org/7.4.2/userguide/command_line_interface.html#sec:command_line_warnings

BUILD SUCCESSFUL in 1s
29 actionable tasks: 2 executed, 27 up-to-date
```
(exit code: 0).

Note: when trying to reproduce, make sure the failure is due to the failed test and not some other reason (such as
missing Java/JDK); failure due to any other reason needs to be fixed first before the buggy behaviour described above
can be observed.
