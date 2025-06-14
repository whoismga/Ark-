<h1 align="center"><b>Ark: Accruing and Reusing Knowledge</b></h1>

We develop **Open Foundation Models** from numerous public datasets using their heterogeneous expert annotations. 

<p align="center"><img width=50% alt="FrontCover" src="Ark_MICCAI2023/media/ark.png"></p>

## Publication

<b>A Fully Open AI Foundation Model Applied to Chest Radiography </b> <br/>
[DongAo Ma](https://www.linkedin.com/in/dongaoma/)<sup>1</sup>, [Jiaxuan Pang](https://www.linkedin.com/in/jiaxuan-pang-b014ab127/)<sup>1</sup>, [Michael B. Gotway](https://www.mayoclinic.org/biographies/gotway-michael-b-m-d/bio-20055566)<sup>2</sup>, [Jianming Liang](https://chs.asu.edu/jianming-liang)<sup>1</sup><br/>
<sup>1 </sup>Arizona State University, <sup>2 </sup>Mayo Clinic <br/>
<b>*Nature* (2025)</b>

[Paper](https://www.nature.com/articles/s41586-025-09079-8) ([PDF](https://rdcu.be/eqx2i), [Supplementary](https://static-content.springer.com/esm/art%3A10.1038%2Fs41586-025-09079-8/MediaObjects/41586_2025_9079_MOESM1_ESM.pdf), [Peer Review](https://static-content.springer.com/esm/art%3A10.1038%2Fs41586-025-09079-8/MediaObjects/41586_2025_9079_MOESM3_ESM.pdf)) | [Github](https://github.com/jlianglab/Ark/tree/main/Ark_Plus) | [CodeOcean](https://codeocean.com/capsule/8456055/tree) for Reprodicible Run

<b>Ark+: Accruing and Reusing Knowledge from Heterogeneous Labels for Superior and Robust Foundation Models </b> 

<b>*Medical Image Analysis* (Accepted, to appear in 2025)</b>


<b>Foundation Ark: Accruing and Reusing Knowledge for Superior and Robust Performance </b> <br/>
[DongAo Ma](https://www.linkedin.com/in/dongaoma/)<sup>1</sup>, [Jiaxuan Pang](https://www.linkedin.com/in/jiaxuan-pang-b014ab127/)<sup>1</sup>, [Michael B. Gotway](https://www.mayoclinic.org/biographies/gotway-michael-b-m-d/bio-20055566)<sup>2</sup>, [Jianming Liang](https://chs.asu.edu/jianming-liang)<sup>1</sup><br/>
<sup>1 </sup>Arizona State University, <sup>2 </sup>Mayo Clinic <br/>
<b>*International Conference on Medical Image Computing and Computer Assisted Intervention ([MICCAI 2023](https://conferences.miccai.org/2023/en/))*</b> (Oral + Poster)

🏆 [Best Paper Award Runner-up](http://www.miccai.org/about-miccai/awards/best-paper-award-and-young-scientist-award/)

★ [MICCAI 2023 STAR Awards](https://conferences.miccai.org/2023/en/MICCAI-2023-STudent-Author-Registration-(STAR)-Awards.html)

[Paper](https://link.springer.com/chapter/10.1007/978-3-031-43907-0_62) ([PDF](https://rdcu.be/dnwdJ), [Arxiv](https://arxiv.org/abs/2310.09507)) | [Code](https://github.com/jlianglab/Ark) | [Poster](Ark_MICCAI2023/media/Ark_poster.pdf) | Oral Presentation ([YouTube](https://youtu.be/-gq1Zl-mh60), [BiliBili](https://www.bilibili.com/video/BV1ww411Y7Yv/))

<p align="left"><img width=40% alt="FrontCover" src="Ark_MICCAI2023/media/BestPaperRunnerUp.JPG"></p>


## Dataset
1. [CheXpert](https://stanfordmlgroup.github.io/competitions/chexpert/)
2. [ChestX-ray14](https://nihcc.app.box.com/v/ChestXray-NIHCC)
3. [RSNA Pneumonia](https://www.kaggle.com/c/rsna-pneumonia-detection-challenge)
4. [VinDrCXR](https://vindr.ai/datasets/cxr)
5. [Shenzhen](https://lhncbc.nlm.nih.gov/LHC-downloads/downloads.html#tuberculosis-image-data-sets)
6. [MIMIC](https://physionet.org/content/mimic-cxr/2.0.0/)


## Pre-trained Ark/Ark+ models

You can request the pretrained Ark and Ark+ models (the teacher model) in our paper throught this [Google Form](https://forms.gle/qkoDGXNiKRPTDdCe8) or [wjx.cn](https://www.wjx.cn/vm/OvwfYFx.aspx#).


### Load the model
1. Create Swin Transformer Base model from the [official model](https://github.com/microsoft/Swin-Transformer/blob/main/models/swin_transformer.py) or from [timm (v0.5.4)](https://github.com/huggingface/pytorch-image-models/tree/main#models):
```
model = timm.create_model('swin_base_patch4_window7_224', num_classes=args.num_class, pretrained=False)
```
If you encounter a _size mismatch_ error when loading a pretrained model, please verify the version of the timm package, as later versions have updated the Swin Transformer architectures. 

2. Load the weight:
```
state_dict = torch.load('<PATH_TO_MODEL>/ark6_teacher_ep200_swinb_projector1376_mlp.pth.tar', map_location="cpu")
for k in ['head.weight', 'head.bias', 'head_dist.weight', 'head_dist.bias']:
    if k in state_dict:
        print(f"Removing key {k} from pretrained checkpoint")
        del state_dict[k] 
model.load_state_dict(state_dict, strict=False)
```
### Finetune the model on target tasks
We have integrated Ark pretrained models in our [Benchmark Tansformers GitHub Repository](https://github.com/jlianglab/BenchmarkTransformers)
```
$ git clone https://github.com/jlianglab/BenchmarkTransformers.git
$ cd BenchmarkTransformers
```
```
python main_classification.py --data_set ChestXray14  
--model swin_base 
--init ark 
--pretrained_weights [PATH_TO_MODEL]/ark6_teacher_ep200_swinb_projector1376_mlp.pth.tar 
--data_dir [PATH_TO_DATASET] 
--train_list dataset/Xray14_train_official.txt 
--val_list dataset/Xray14_val_official.txt 
--test_list dataset/Xray14_test_official.txt 
--lr 0.01 --opt sgd --epochs 200 --warmup-epochs 0 --batch_size 64
```

## Citation
If you use this code or use our pre-trained weights for your research, please cite our paper:
```
@InProceedings{ma2023foundation,
    author="Ma, DongAo and Pang, Jiaxuan and Gotway, Michael B. and Liang, Jianming",
    title="Foundation Ark: Accruing and Reusing Knowledge for Superior and Robust Performance",
    booktitle="Medical Image Computing and Computer Assisted Intervention -- MICCAI 2023",
    year="2023",
    publisher="Springer Nature Switzerland",
    address="Cham",
    pages="651--662",
    isbn="978-3-031-43907-0"
}
```

## Acknowledgement
This research has been supported in part by ASU and Mayo Clinic through a Seed Grant and an Innovation Grant, and in part by the NIH under Award Number R01HL128785. The content is solely the responsibility of the authors and does not necessarily represent the official views of the NIH. This work has utilized the GPUs provided in part by the ASU Research Computing and in part by the Bridges-2 at Pittsburgh Supercomputing Center through allocation BCS190015 and the Anvil at Purdue University through allocation MED220025 from the Advanced Cyberinfrastructure Coordination Ecosystem: Services & Support (ACCESS) program, which is supported by National Science Foundation grants #2138259, #2138286, #2138307, #2137603, and #2138296. We also acknowledge Google for granting us access to CXR Foundation API, which enabled us to generate the embeddings for the target datasets. The content of this paper is covered by patents pending.


## License

Released under the [ASU GitHub Project License](./LICENSE).
