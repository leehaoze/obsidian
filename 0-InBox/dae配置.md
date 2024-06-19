---
date created: 星期三, 六月 19日 2024, 8:39:38 晚上
date modified: 星期三, 六月 19日 2024, 9:09:25 晚上
tags: 
---

# Dae配置

我们将dae的配置分散到各个独立的文件单独维护，这样对于经常调整的部分单独维护，可以在调试时快速更换不同的配置文件。

## 分散配置

分散配置文件参考[Separate Configuration Files](https://github.com/daeuniverse/dae/blob/main/docs/en/configuration/separate-config.md)。

- 创建配置子目录

```shell
mkdir config.d
```

- 在主配置文件中仅保留如下内容：

```yaml
include {
	config.d/*.dae
}
```

## Global配置

```

```