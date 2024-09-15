# BundleAdjustment代码阅读




## 地图点：Map Point


### 地图点和key frame的观测关系

对于某个地图点pMp，可以通过调用GetObservations()方法，获取其在keyframe中的观测，就是不同keyframe对这个地图点的观测索引（keypoint index），
可能这个点被多个key frames观测到，因此，这里得到的是一个map<KeyFrame*,tuple<int,int>>。

![image](https://github.com/user-attachments/assets/2abda798-a9d9-4e3f-a053-2ee045b9ad93)

如果是单目相机，tuple中第一个int有效，第二个int是一个负值。
```cpp
const map<KeyFrame*, tuple<int,int>> observations = pMP->GetObservations();
```

在BundleAdjustment中，有相应的处理，核心代码如下：

```cpp
void Optimizer::BundleAdjustment(const vector<KeyFrame *> &vpKFs,
                                 const vector<MapPoint *> &vpMP, ...)
{
    for (size_t i = 0; i < vpMP.size(); i++)
    {
        MapPoint* pMP = vpMP[i];
        // all the observations of this map point
        const map<KeyFrame*,tuple<int,int>> observations = pMP->GetObservations();

        for (auto mit = observations.begin(); mit != observations.end(); ++mit)
        {
            KeyFrame* pKF = mit->first;
            const int leftIndex = get<0>(mit->second);
            const cv::KeyPoint &kpUn = pKF->mvKeysUn[leftIndex];
            Eigen::Matrix<double, 2, 1> obs << kpUn.pt.x, kpUn.pt.y;
            ...
            e->setMeasurement(obs);
        }
        ...
    }
    ...
}
```

