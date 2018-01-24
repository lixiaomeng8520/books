# 入门

这篇文章让你开始在TensorFlow里进行编程。开始学习之前，请安装TensorFlow。在这篇指南之外，你应该了解下面的知识：
1. 使用Python。
2. 了解数组。
3. 最少了解一些机器学习。然而，如果你完全不懂机器学习，这篇文章仍然适合你。

TensorFlow提供很多API。最低级的API是TensorFlow Core，提供你完整的编程控制。我们推荐TensorFlow Core用于机器学习研究人员和其他需要对其模型进行精细控制的人员。高级API是基于TensorFlow Core构建。高级API易于学习和使用。另外，高级API使重复性的工作更加简单，而且在不同用户之间表现一致。比如tf.estimator可以帮助你管理数据集、评估、训练和泛化。

下面先从TensorFlow Core开始。之后，我们再讲如何用tf.estimator来实现一个相同的模型。了解TensorFlow Core的原理将使你在使用高级API时，更好的了解内部机制。