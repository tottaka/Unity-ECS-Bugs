[Source](https://forum.unity.com/threads/2020-3-16f1-physics-world-history-objectdisposedexception-cannot-access-a-disposed-object.1158956/#post-7525850)
## Fix Description
  Change AsDeferredJobArray allocator from None to Invalid.  

## Solution
1. Navigate to `<project_root>/Library/PackageCache/`.
2. Copy the `com.unity.collections@0.15.0-preview.21` package into `<project_root>/Packages/` so the PackageManager doesn't override the changes you will make.
4. Inside the copied package directory, open the file `/Unity.Collections/NativeList.cs`
5. Change line 599 from `Allocator.None` to `Allocator.Invalid`.

## Related Bugs
1. [PhysicsWorldHistory](../../NetCode/PhysicsWorldHistory.md) (com.unity.netcode)

## Error Stack
```
IndexOutOfRangeException: Index 0 is out of range of '0' Length.
Unity.Collections.NativeArray`1[T].FailOutOfRangeError (System.Int32 index) (at <d3b66f0ad4e34a55b6ef91ab84878193>:0)
Unity.Collections.NativeArray`1[T].CheckElementReadAccess (System.Int32 index) (at <d3b66f0ad4e34a55b6ef91ab84878193>:0)
Unity.Collections.NativeArray`1[T].get_Item (System.Int32 index) (at <d3b66f0ad4e34a55b6ef91ab84878193>:0)
Unity.NetCode.GhostSendSystem+SerializeJob.Execute (System.Int32 idx, Unity.Entities.DynamicComponentTypeHandle* ghostChunkComponentTypesPtr, System.Int32 ghostChunkComponentTypesLength) (at Packages/com.unity.netcode@0.6.0-preview.7/Runtime/Snapshot/GhostSendSystem.cs:777)
Unity.NetCode.GhostSendSystem+SerializeJob32.Execute (System.Int32 idx) (at Packages/com.unity.netcode@0.6.0-preview.7/Runtime/Snapshot/GhostSendSystem.cs:673)
Unity.NetCode.IJobParallelForDeferRefExtensions+JobParallelForDeferProducer`1[T].Execute (T& jobData, System.IntPtr additionalPtr, System.IntPtr bufferRangePatchData, Unity.Jobs.LowLevel.Unsafe.JobRanges& ranges, System.Int32 jobIndex) (at Packages/com.unity.netcode@0.6.0-preview.7/Runtime/Snapshot/GhostSendSystem.cs:58)
```

```
System.IndexOutOfRangeException: Index {0} is out of range of '{1}' Length.
Thrown from job: Unity.NetCode.GhostSendSystem.SerializeJob32
This Exception was thrown from a job compiled with Burst, which has limited exception support. Turn off burst (Jobs -> Burst -> Enable Compilation) to inspect full exceptions & stacktraces
```
