# 🚀 QR-LoRA 🚀

**Official PyTorch implementation of QR-LoRA**

[QR-LoRA: Efficient and Disentangled Fine-tuning via QR Decomposition for Customized Generation](https://arxiv.org/abs/2507.04599)  

##  🌼 Abstract
We propose QR-LoRA, a novel fine-tuning framework leveraging QR decomposition for structured parameter updates that effectively separate visual attributes. Our key insight is that the orthogonal Q matrix naturally minimizes interference between different visual features, while the upper triangular R matrix efficiently encodes attribute-specific transformations. Our approach fixes both Q and R matrices while only training an additional task-specific ΔR matrix. This structured design reduces trainable parameters to half of conventional LoRA methods and supports effective merging of multiple adaptations without cross-contamination due to the strong disentanglement properties between ΔR matrices.

![QR-LoRA](static/images/qrlora-method1.png)


## 🎯 Key Features in QR-LoRA

- **🔄 Superior Disentanglement**: Achieves superior disentanglement in content-style fusion tasks through orthogonal decomposition
- **⚡ Parameter Efficiency**: Reduces trainable parameters to half of conventional LoRA methods
- **🔧 Easy Integration**: Simple element-wise addition for merging multiple adaptations without cross-contamination
- **🚀 Fast Convergence**: Enhanced initialization strategy enables faster convergence when training both Q and R matrices
- **🎨 Flexible Control**: Fine-grained control over content and style features through scaling coefficients

## 🛠️ Installation

```bash
git clone https://github.com/luna-ai-lab/QR-LoRA.git
cd QR-LoRA
conda create -n qrlora python=3.10 -y
conda activate qrlora
pip install -r requirements.txt
```

## 🚀 Quick Start

### Train QR-LoRA for a given input image based on [FLUX-dev1](https://huggingface.co/black-forest-labs/FLUX.1-dev):

```bash
# for style:
bash flux_dir/train_deltaR_sty.sh 0 64

# for content:
bash flux_dir/train_deltaR_cnt.sh 0 64

# for fast convergence in one task:
bash flux_dir/train_QR.sh 0 64
```

#### Inference:
To reduce inference time overhead, we recommend pre-saving the initialization decomposition matrices. This eliminates the need to perform initialization decomposition during each inference:
```bash
bash flux_dir/save_flux_residual.sh 1
```
After executing the above script, a model file `flux_residual_weights.safetensors` will be generated in the `flux_dir` directory. Alternatively, you can also download it directly from [🤗flux_res](https://huggingface.co/yjh001/flux_res). Then, configure the corresponding training model path and execute the inference script:
```bash
bash flux_dir/inference_merge.sh 1
```

#### Similarity analysis:
```bash
bash test/visualize_qrlora_similarity.sh 1
```


## TODO
- [ ] Add SDXL-based QR-LoRA training and inference scripts
- [ ] Release pre-trained QR-LoRA model weights
- [ ] Provide more tutorials and application cases


## Limitations
Disentanglement is not a sufficient and necessary condition for good LoRA merging. While good merging results can imply disentanglement properties, having disentanglement properties does not always guarantee good merging performance.


## 🤝 Acknowledgements
We are deeply thankful to the authors of the following works, which have significantly inspired our ideas and methodologies. 

- [ZipLoRA](https://ziplora.github.io/)

- [B-LoRA](https://b-lora.github.io/B-LoRA/)

- [PiSSA](https://arxiv.org/abs/2404.02948)

- [HydraLoRA](https://github.com/Clin0212/HydraLoRA)

We also gratefully acknowledge the open-source libraries like [diffusers](https://huggingface.co/docs/diffusers/index), [transformers](https://huggingface.co/docs/transformers/index), and [accelerate](https://huggingface.co/docs/accelerate/index) that made our research possible.

**We hope that the elegant simplicity of our QR-LoRA approach will inspire further research in your domain!**



## 📄 Citation
```
@inproceedings{yang2025qrlora,
  title={QR-LoRA: Efficient and Disentangled Fine-tuning via QR Decomposition for Customized Generation},
  author={Jiahui Yang and Yongjia Ma and Donglin Di and Hao Li and Wei Chen and Yan Xie and Jianxun Cui and Xun Yang and Wangmeng Zuo},
  booktitle=International Conference on Computer Vision,
  year={2025}
}
```