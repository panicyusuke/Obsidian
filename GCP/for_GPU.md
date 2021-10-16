#GCP 

# GPUを有効にする

AI Platformでは複数のGPUマシンタイプを選択できるが、`YoLov5`では最も軽量なT4に設定すると良い。

```python

request_dict = {
    'name': 'VERSION_NAME',
    'deploymentUri': 'gs://model-directory-uri',
    'runtimeVersion': '2.5',
    'pythonVersion': '3.7',
    'framework': 'TENSORFLOW',
    'machineType': 'n1-standard-4',
    'acceleratorConfig': {
      'count': 1,
      'type': 'NVIDIA_TESLA_T4'
    },
    'autoScaling': {
      'minNodes': 2,
      'maxNodes': 4,
      'metrics': [
        {
          'name': 'GPU_DUTY_CYCLE',
          'target': 60
        }
      ]
    }
}

```

cf. [オンライン予測用のマシンタイプの選択](https://cloud.google.com/ai-platform/prediction/docs/machine-types-online-prediction?hl=ja)