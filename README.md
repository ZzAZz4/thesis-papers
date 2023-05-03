# thesis
<!-- overleaf -->
[Overleaf](https://www.overleaf.com/2246477953kyvmgjxmqgvb)

## Papers

| Paper | Code | Summary or whatever I understood | Observations |
| :---: |  :--: | :----------: | :-: |
| [**`POCO: Point Convolution for Surface Reconstruction`**](https://arxiv.org/abs/2201.01831) | [**`GitHub`**](https://github.com/valeoai/POCO) | Learns per-point latent codes via an encoder. Then feeds extended neighbor encodings alongside distance vectors to a query point to estimate occupancy. The reconstructed model is just the bounding surface, infered via marching cubes. | Gets destroyed by dense uniform noise as it depends on KNN. The attention trick to infer significance is interesting, though missing regions just get ignored as neighbor significance decreases. Details are retained, and it works on scenes. |
| [**`DeepSDF: Learning Continuous Signed Distance Functions for Shape Representation`**](https://arxiv.org/abs/1901.05103) | [**`GitHub`**](https://github.com/facebookresearch/DeepSDF) | Learns per-point latent codes via an autodecoder network. Then feeds an extended query point to a neural network to estimate an SDF. The reconstructed model is just the isosurface encoded at zero, infered via marching cubes. | Not equivariant network at the autodecoder. SDF's still can't represent very thin/complex shapes by nature. Missing regions and noise may potentially be handled during the encoding step, though it is not shown. Normal consistency is handled on SDF level. |
| [**`Reconstructing Surfaces for Sparse Point Clouds with On-Surface Priors`**](https://arxiv.org/abs/2204.10603) | [**`GitHub`**](https://github.com/mabaorui/OnSurfacePrior) | Learn the SDF, project query points into the isosurface using it and then use an on-surface prior (id. info from neighbors) to determine if a point is on the same surface patch as their adjacent points. | Handles sparsity very nicely. The implementation depends on a per-scene density-specific hyperparameter. Learns using only the point cloud without normals and distances. The prior requires normalization as it is influenced by 3D transforms, especially translations, so not equivariant. Cannot handle missing regions, though suggests a shape completion prior. |
| [**`Surface Reconstruction from Point Clouds: A Survey and a Benchmark`**](https://arxiv.org/abs/2205.02413) | [**`GitHub`**](https://github.com/Gorilla-Lab-SCUT/SCUTSurface-code) | A survey of surface reconstruction methods. Contains a supermassive dataset for testing models and generalization under every single bad condition possible. Mentions basically every state-of-the-art method until then. | Pretty complete. The dataset is pretty helpful. It's dataset filters complex and non-watertight shapes, although that may be reasonable because every previous method prolly doesn't work there. Apparently it also shows that learning-based methods fail on generalizing and get destroyed sometimes. It also empirically shows that missing regions is the most problematic scanning imperfection. Doesn't necessarily use the most optimal method for every prior, only the most known one. |
| [**`GenSDF: Two-Stage Learning of Generalizable Signed Distance Functions`**](https://light.princeton.edu/publication/gensdf/) | [**`GitHub`**](https://github.com/princeton-computational-imaging/gensdf) | Uses a semi-supervised approach to learn SDFs. It uses both labeled distances and a self-supervised loss formulation with unlabeled data in order to achieve generalization. Shows good results with unseen classes. | Given that the survey showed that generalization doesn't happen with other methods, it's good that this one focuses on that specifically. The self-supervised loss is simply dependant on the closest point, so noise and missing data may be problematic, the former being shown empirically for gaussian noise, and the effect may be worsened if noise is uniform.
| [**`ScanComplete: Large-Scale Scene Completion and Semantic Segmentation for 3D Scans`**](https://arxiv.org/pdf/1712.10215.pdf) | [**`GitHub`**](https://github.com/angeladai/ScanComplete) | Learns to complete Truncated SDFs and predicting semantics in a coarse-to-fine manner, by recursively feeding the reconstructed SDF and semantics into a 3D-CNN alongside a better sampled partial scan voxel group each iteration. | Coarse-to-fine is interesting. It depends on an SDF and not a point cloud, so a conversion is necessary. Semantics don't actually matter for completion, which is useful because they may not appear everywhere and I don't care about them anyways. Resolution may not be the best to preserve details, though that may be improved by just adding more resolution layers. Works on whole scenes. |
| [**`Neural Unsigned Distance Fields for Implicit Function Learning`**](https://virtualhumans.mpi-inf.mpg.de/papers/chibane2020ndf/chibane2020ndf.pdf) | [**`GitHub`**](https://github.com/jchibane/ndf) | Employs a similar method as the original SDF, but for infering Unsigned Distance Fields. Renders the model via ray tracing. It also allows for easy consolidation and upsampling. | Can represent very complex, thin shapes and basically whatever because of the unsigned representation, which doesn't require an inside/outside boundary. Doesn't show how to reconstruct them into meshes, only how to render them. Normals are no longer trivial, though can still be infered. The field is not guaranteed to be a true distance field, which is also true for the ray tracing step as it requires dampening. |
| [**`Learning Consistency-Aware Unsigned Distance Functions Progressively from Raw Point Clouds`**](https://arxiv.org/pdf/2210.02757.pdf) | [**`GitHub`**](https://github.com/junshengzhou/CAP-UDF) | Learns an UDF via overfitting the raw point cloud. It uses a different loss function to guarantee consistency alongside the UDF path. It also proposes a way to actually get a mesh from the UDF via a modified marching cubes.  | No dampening is needed for close points because of the consistency, although very distant query points do show problems in their experimentation. Recommends using a coarse-to-fine approach for reconstruction, as the method is slow and may still require successive runs for further points. |

