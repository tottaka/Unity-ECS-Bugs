[Source](https://forum.unity.com/threads/2020-3-16f1-physics-world-history-objectdisposedexception-cannot-access-a-disposed-object.1158956/#post-7525850)
## Fix Description
  Remove deprecated ENABLE_UNITY_COLLECTIONS_CHECKS check. The NumBodies API cannot be safely used anymore.

## Solution
1. Navigate to `<project_root>/Library/PackageCache/`.
2. Copy the `com.unity.netcode@0.6.0-preview.7` package into `<project_root>/Packages/` so the PackageManager doesn't override the changes you will make.
4. Inside the copied package directory, open the file `/Runtime/Physics/PhysicsWorldHistory.cs`
5. Remove ```#if ENABLE_UNITY_COLLECTIONS_CHECKS
  if (m_buffer.GetWorldAt(index).NumBodies > 0)
  {
      UnityEngine.Debug.LogError("Not disposing CollisionWorld before assign a new one might cause memory leak");
  }
#endif``` check starting on line 191.

## Related Bugs
1. [AsDeferredJobArray](../Collections/NativeList/AsDeferredJobArray.md)

## Error Stack
```
ObjectDisposedException: Cannot access a disposed object.
Object name: 'The NativeArray has been disposed, it is not allowed to access it'.
Unity.Collections.LowLevel.Unsafe.AtomicSafetyHandle.CheckExistsAndThrow (Unity.Collections.LowLevel.Unsafe.AtomicSafetyHandle& handle) (at <d3b66f0ad4e34a55b6ef91ab84878193>:0)
Unity.Collections.LowLevel.Unsafe.AtomicSafetyHandle.ValidateNonDefaultHandle (Unity.Collections.LowLevel.Unsafe.AtomicSafetyHandle& handle) (at <d3b66f0ad4e34a55b6ef91ab84878193>:0)
Unity.Collections.NativeArray`1[T].get_Length () (at <d3b66f0ad4e34a55b6ef91ab84878193>:0)
Unity.Physics.Broadphase+Tree.get_NumBodies () (at Library/PackageCache/com.unity.physics@0.6.0-preview.3/Unity.Physics/Collision/World/Broadphase.cs:371)
Unity.Physics.Broadphase.get_NumStaticBodies () (at Library/PackageCache/com.unity.physics@0.6.0-preview.3/Unity.Physics/Collision/World/Broadphase.cs:28)
Unity.Physics.CollisionWorld.get_NumBodies () (at Library/PackageCache/com.unity.physics@0.6.0-preview.3/Unity.Physics/Collision/World/CollisionWorld.cs:23)
Unity.NetCode.CollisionHistoryBuffer.CloneCollisionWorld (System.Int32 index, Unity.Physics.CollisionWorld& collWorld) (at Library/PackageCache/com.unity.netcode@0.6.0-preview.7/Runtime/Physics/PhysicsWorldHistory.cs:192)
Unity.NetCode.PhysicsWorldHistory.OnUpdate () (at Library/PackageCache/com.unity.netcode@0.6.0-preview.7/Runtime/Physics/PhysicsWorldHistory.cs:349)
Unity.Entities.SystemBase.Update () (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/SystemBase.cs:412)
Unity.Entities.ComponentSystemGroup.UpdateAllSystems () (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystemGroup.cs:472)
UnityEngine.Debug:LogException(Exception)
Unity.Debug:LogException(Exception) (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/Stubs/Unity/Debug.cs:19)
Unity.Entities.ComponentSystemGroup:UpdateAllSystems() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystemGroup.cs:477)
Unity.Entities.ComponentSystemGroup:OnUpdate() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystemGroup.cs:417)
Unity.Entities.ComponentSystem:Update() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystem.cs:114)
Unity.Entities.ComponentSystemGroup:UpdateAllSystems() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystemGroup.cs:472)
Unity.Entities.ComponentSystemGroup:OnUpdate() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystemGroup.cs:417)
Unity.NetCode.ServerSimulationSystemGroup:OnUpdate() (at Library/PackageCache/com.unity.netcode@0.6.0-preview.7/Runtime/ClientServerWorld/ServerSimulationSystemGroup.cs:75)
Unity.Entities.ComponentSystem:Update() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystem.cs:114)
Unity.Entities.ComponentSystemGroup:UpdateAllSystems() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystemGroup.cs:472)
Unity.Entities.ComponentSystemGroup:OnUpdate() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystemGroup.cs:417)
Unity.Entities.ComponentSystem:Update() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystem.cs:114)
Unity.Entities.ComponentSystemGroup:UpdateAllSystems() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystemGroup.cs:472)
Unity.Entities.ComponentSystemGroup:OnUpdate() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystemGroup.cs:417)
Unity.Entities.ComponentSystem:Update() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ComponentSystem.cs:114)
Unity.Entities.DummyDelegateWrapper:TriggerUpdate() (at Library/PackageCache/com.unity.entities@0.17.0-preview.42/Unity.Entities/ScriptBehaviourUpdateOrder.cs:333)
```
