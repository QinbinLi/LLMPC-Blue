# Starting Kit for LLMPC Blue Team
This repository is for the Blue Team participants for [NeurIPS 2024 LLM Privacy Challlenge](https://llm-pc.github.io/).

## Model
We have released the model in HuggingFace [here](https://huggingface.co/LLM-PBE/LLMPC-Blue-Team-Llama3.1-8b-instruct), which is the Llama3.1-8b-instruct model fine-tuned on chat data with private information.

## Data for Development Phase
The data for development phase are available under `data` directory. There are two files
- LLM-PC-development-scrubbed-data.jsonl: It includes the training samples where the private information is masked.
- LLM-PC-development-pii.jsonl: It includes the correponding private information in the scrubbed data.

## Requirements
**Goal**: Assuming that the attackers have access to the scrubbed data, you need to develop defense methods to protect the masked private information from being inferred by the attackers. We will use attack methods to evaluate the effectiveness of your solution. The provided data is for your reference in the development phase.

**Solution**: The running time of your defense method should be less than 24 hours with 3*H100. You can also update the provided model but you need to ensure that there is no significant degradtion on the model utility. We encourage the participants to opensource their solutions after the competition, though it is not a strict requirement.

**Submission**: You will be required to submit 
1. A short paper that briefly describes your solution and results (e.g., changes on model utility and attack success rate, efficiency). The template is available [here](https://github.com/QinbinLi/LLMPC-Blue/blob/main/LLMPC-Submission-Template.zip). The main paper is limited to **four content pages**. Additional pages containing references and appendices are allowed;
2. Your source code and model (if any). In your source code, you need to provide function `def query(prompt="xxx")` in `main.py`, which support querying the LLM with your proposed defense method. Experts will review your paper and provide their ratings.

> [!IMPORTANT]
> __Please email your paper and code to <llmpc2024.info@gmail.com> by Nov 1st AOE.__


## Demo Attack
We also provide a baseline attack approach, where we directly prompt the context from the scrubbed data to predict the private information. You can run it by the following instructions
```
git clone https://github.com/QinbinLi/LLM-PBE.git
cd LLM-PBE
conda create -n llm-pbe python=3.10 -y
conda activate llm-pbe
pip install -r requirements.txt
python -m attacks.DataExtraction.llm_pc_attack_baseline --model LLM-PBE/Llama3.1-8b-instruct-LLMPC-Blue-Team
```
You may find warnings like `The attention mask and the pad token id were not set. xxx`, which is normal. You can find results like `ASR (Attack Success Rate): 4.01% (775/19337)` in the output. Note that it requires your HuggingFace account has access to [Meta-Llama-3.1-8B-Instruct](https://huggingface.co/meta-llama/Meta-Llama-3.1-8B-Instruct).

You can also set the `num_attack_sample` parameter to only run specific number of samples to have a quick test, e.g.,
```
python -m attacks.DataExtraction.llm_pc_attack_baseline --model LLM-PBE/Llama3.1-8b-instruct-LLMPC-Blue-Team --num_attack_sample 1000
```

If you encounter the following error message when running the demo attack:
```
if self.pad_token_id is not None and self.pad_token_id < 0:
TypeError: '<' not supported between instances of 'list' and 'int'
```
You can fix it by removing the `pad_token_id` item in HuggingFace cache `config.json` (e.g., the path may be like `~/.cache/huggingface/hub/models--LLM-PBE--Llama3.1-8b-instruct-LLMPC-Blue-Team/snapshots/xxx/config.json`) and run again.

## Contact
If you have any question, please create an new issue or contact <llmpc2024.info@gmail.com>.

## Citation
The attack demo is based on LLM-PBE. If you find it useful, please cite our paper.

```
@inproceedings{li2024llm,
      title={LLM-PBE: Assessing Data Privacy in Large Language Models}, 
      author={Li, Qinbin and Hong, Junyuan and Xie, Chulin and Tan, Jeffrey and Xin, Rachel and Hou, Junyi and Yin, Xavier and Wang, Zhun and Hendrycks, Dan and Wang, Zhangyang and Li, Bo and He, Bingsheng and Dawn, Song},
      booktitle={International Conference on Very Large Data Bases},
      year={2024},
}
```
