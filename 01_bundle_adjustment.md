# BundleAdjustment代码阅读




## 地图点：Map Point


### 地图点和key frame的观测关系

对于某个地图点pMp，可以通过调用`GetObservations()`方法，获取其在keyframe中的索引，可能这个点被多个key frames观测到，
因此，这里得到的是一个`map<KeyFrame*,tuple<int,int>>`。

如果是单目相机，tuple中第一个int有效，第二个int是一个负值。
```cpp
const map<KeyFrame*,tuple<int,int>> observations = pMP->GetObservations();
```
我们看到，tuple中第二个camera得到的是一个负数。

```cpp

void Optimizer::BundleAdjustment(const vector<KeyFrame *> &vpKFs,
                                 const vector<MapPoint *> &vpMP, ...)
{
    for (size_t i = 0; i < vpMP.size(); i++)
    {
        MapPoint* pMP = vpMP[i];
        // the the point index of the map point from the keyframe
        const map<KeyFrame*,tuple<int,int>> observations = pMP->GetObservations();

    }






}


```

