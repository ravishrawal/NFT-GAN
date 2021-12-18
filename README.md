# NFT-GAN

**Project Description:**
- Fine-tuning GANs is a good way to save time and computation, especially when our target dataset is small (1-10k images)
- “GANs are notoriously tricky to train (Salimans et al., 2016)” - fine-tuning is also tricky because GANs overfit very easily, and there are a multitude of pre-trained models to start training from
- We experiment with a new technique called freezeD (freezing the first D layers of the discriminator) to fine-tune StyleGAN2-ADA to a new dataset we created called BoredApes and find the best freezeD configuration is when freezeD=10. 
- StyleGAN2-ADA fine-tuned on the BoredApe dataset rather quickly - it could generate reasonable images in only 1K iterations as opposed to millions when training from scratch
- We propose two new metrics: inter-dataset similarity (IDS) and intra-dataset diversity (IDD) to explore the relationship between the source and target dataset, and whether they correlate with convergence to a target KID


**Repo Guide:**


The repo consists mostly of Jupyter Notebooks, each with a specific function:

- selenium_scraper.ipynb: Code to scrape target dataset images using infinite scroll & user-agent spoofing
- freezed-experiment.ipynb: Code with examples of how to run training on Stylegan2
- Plots.ipynb: Code to generate our visualization
- Dataset_IDS_ID.ipynb: Code to calculate our IDS & IDD metrics on the datasets


**Running Code:**

Follow the jupyter notebooks to run code. 


Fine Tuning StyleGAN with FreezeD: 
```
python train.py --outdir=training-runs --data=boredape-small-256x256.zip --gpus=1 --batch=32 --mirror=0 --resume=celebahq256 --gamma=2 --kimg=1000 --snap=10 --metrics=none --freezed=10
```

Calculate IDS:
```
python3 calc_metrics.py --metrics=kid50k_full --data1=../datasets/ffhq256.zip --data2=../datasets/boredape-small-256x256.zip
```
Calculate IDD:
```
calc_diversity(ds_dir=os.path.join(celeb_dir,'celeba_hq_256'), rand_sample_size=1000, padding = 25)
```

Results:
- We find that KID results are more varied at convergence when datasets are less similar, implying that we can afford to freeze more D layers when the IDS score is high.
- IDD results are inconclusive

![alt text](https://github.com/ravishrawal/NFT-GAN/blob/main/GAN_output.png?raw=true)

![alt text](https://github.com/ravishrawal/NFT-GAN/blob/main/GAN_plots.png?raw=true)

