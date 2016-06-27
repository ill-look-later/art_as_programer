# bullshit courage && fucking life



36 tests failed:
---

    ConditionVariableTest.LargeFastTaskTest
    ConditionVariableTest.MultiThreadConsumerTest
    ConditionVariableTest.TimeoutTest
    FileUtilTest.ChangeDirectoryPermissionsAndEnumerate
    FileUtilTest.ChangeFilePermissionsAndRead
    FileUtilTest.ChangeFilePermissionsAndWrite
    FileUtilTest.GetShmemTempDirTest
    JSONFileValueSerializerTest.NoWhitespace
    JSONFileValueSerializerTest.Roundtrip
    JSONFileValueSerializerTest.RoundtripNested
    JSONReaderTest.ReadFromFile
    JsonPrefStoreTest.AlternateFile
    JsonPrefStoreTest.AlternateFileDNE
    JsonPrefStoreTest.AlternateFileIgnoredWhenMainFileExists
    JsonPrefStoreTest.AsyncNonExistingFile
    JsonPrefStoreTest.Basic
    JsonPrefStoreTest.BasicAsync
    JsonPrefStoreTest.BasicAsyncWithAlternateFile
    JsonPrefStoreTest.InvalidFile
    JsonPrefStoreTest.NonExistentFile
    JsonPrefStoreTest.NonExistentFileAndAlternateFile
    JsonPrefStoreTest.PreserveEmptyValues
    JsonPrefStoreTest.ReadAsyncWithInterceptor
    JsonPrefStoreTest.ReadWithInterceptor
    JsonPrefStoreTest.RemoveClearsEmptyParent
    PathServiceTest.Get
    ProcessUtilTest.GetAppOutput
    ProcessUtilTest.GetAppOutputRestricted
    ProcessUtilTest.GetAppOutputRestrictedNoZombies
    ProcessUtilTest.GetAppOutputRestrictedSIGPIPE
    ProcessUtilTest.GetAppOutputWithExitCode
    ProcessUtilTest.LaunchProcess
    ReadOnlyFileUtilTest.ContentsEqual
    ReadOnlyFileUtilTest.TextContentsEqual
    SequencedWorkerPoolTest.DelayedTaskDuringShutdown
    UnixDomainSocketTest.SendRecvMsgAbortOnReplyFDClose
4 tests timed out:
---

    PosixDynamicThreadPoolTest.Basic
    PosixDynamicThreadPoolTest.Complex
    PosixDynamicThreadPoolTest.ReuseIdle
    PosixDynamicThreadPoolTest.TwoActiveTasks
7 tests crashed:
---

    HistogramDeathTest.BadRangesTest
    ScopedFD.ScopedFDCrashesOnCloseFailure
    SecurityTest.CallocOverflow
    StringPrintfTest.PositionalParameters
    SysStrings.SysNativeMBAndWide
    SysStrings.SysNativeMBToWide
    SysStrings.SysWideToNativeMB
Tests took 1233 seconds.
