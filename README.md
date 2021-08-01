# CoLL - ***Co***ntinual ***L***anguage ***L***earning
[![PyPI](https://img.shields.io/pypi/v/coll)](https://pypi.org/project/coll/) [![Documentation](https://img.shields.io/badge/docs-CoLL-blue)](https://wutong8023.site/CoLL/)

A collection of extensions and data-loaders for continual language learning in [PyTorch](https://pytorch.org/). CoLL contains popular continual language learning benchmarks, similarly compatible with both [`Avalanche`](https://github.com/ContinualAI/avalanche) and [`Torchmeta`](https://github.com/tristandeleu/pytorch-meta/).

See [documentation](https://wutong8023.site/CoLL) for more details.


#### Features
  - **Application**: Unified interfaces of typical continual-language-learning applications, including text classification, text generation, and sequence labelling, which enables easy benchmarking on multiple problems and reproducibility.
  - **Learning Paradigm**: Simulating the learning paradigm of full-supervision, semi-supervision, un-supervision or self-supervision.
  - **Continual Setting**: Built-in typical continual learning settings, e.g., instance-incremental learning, class-incremental learning, task-incremental learning, domain-incremental learning. 
  - **Backbone Model**: Supporting various pretrained language models (HuggingFace/Transformers) and the extension modules (Adapters) for continual learning. 
  - **Metrics**: Unified metrics for the fair and systematical comparison.
  - **Baselines**: Built-in implementations and helper functions for some popular methods, with default arguments from the literature.

**Note**: This is still very much a Work-In-Progress! Please feel free to share your wisdom.

## Installation
### Using `pip`
```bash
pip install coll
```
### From Source
```bash
git clone https://github.com/wutong8023/CoLL.git
cd CoLL
python setup.py install
```

## Example
```python
from coll.backbone import PLMClassifier
from coll.environment import Environment, CreEnv
from coll.environment.dataset import FewRel # 
from coll.environment.paradigm import SemiSuper # LimitedSuper, FullSuper, UnSuper, InteractSuper
from coll.environment.setting import TaskIL # ClassIL, InstanceIL, DomainIL
from coll.method import ER # EWC, LwF, LAMOL, MbPA++, etc.
from coll.utils.metrics import acc_a # average accuracy
from coll.utils.buffer_memory import ReservoirMemory
from coll.utils.train import Trainer
from coll.utils.eval import Evaluater

# 1. define continual learning environment
# customize continual learning environment
data = FewRel()
paradigm = SemiSuper()
setting = TaskIL(split_by="clustering")
cl_env = Environment(data, paradigm, setting)

# or load predefined environment
cl_env = CreEnv()

# 2 define backbone model
backbone = PLMClassifier()

# 3 define continual learning strategy
memory = ReservoirMemory(size=500, extend=False)
cl_method = ER(memory)

# 4 train
Trainer.train(backbone, cl_env, cl_method)

# 5 evaluation
results = Evaluater.evaluate(backbone, cl_env, cl_method, acc_a)

print(results.summary())
```


