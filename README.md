# Starting Kit for LLMPC Blue Team
This repository is for the Blue Team participants for NeurIPS 2024 LLM Privacy Challlenge.

## Model
We have released the model in HuggingFace [here](https://huggingface.co/LLM-PBE/LLMPC-Blue-Team-Llama3.1-8b-instruct), which is the Llama3.1-8b-instruct model fine-tuned on chat data with private information.

## Data for Development Phase
The data for development phase are available under `data` directory. There are two files
- LLM-PC-development-scrubbed-data.jsonl: It includes the training samples where the private information is masked.
- LLM-PC-development-pii.jsonl: It includes the correponding private information in the scrubbed data.

## Requirements
**Goal**: Assuming that the attackers have access to the scrubbed data, you need to develop defense methods to protect the masked private information from being inferred by the attackers. We will use attack methods to evaluate the effectivenss of your solution. The provided data is for your reference in the development phase. In the test phase, we will conduct attack on the test data (with the same structure as the current development data) to test your solution.

**Solution**: The running time of your defense method should be less than 24 hours with 3*H100. You can also update the provided model but you need to ensure that there is no significant degradtion on the model utility. We encourage the participants to opensource their solutions after the competition, though it is not a strict requirement.

**Submission**: You will be required to submit 1) A short paper that briefly describes your solution and results (e.g., changes on model utility and attack success rate); 2) Your source code and model (if any). Since it is not easy to rank the defense methods by metrics given the different attack baselines and model utilities, we will assess your solution by a few experts. More information will be provided later.


## Demo Attack
We also provide a baseline attack approach for your reference. You can run it by the following instructions
```
git clone https://github.com/QinbinLi/LLM-PBE.git
conda create -n llm-pbe python=3.10 -y
conda activate llm-pbe
pip install -r requirements.txt
python -m attacks.DataExtraction.llm_pc_attack_baseline --model LLM-PBE/LLMPC-Blue-Team-Llama3.1-8b-instruct
```
You can find `ASR (Attack Success Rate): 2.46% (475/19337)` in the output.

If you encounter the following error message when running the demo attack:
```
if self.pad_token_id is not None and self.pad_token_id < 0:
TypeError: '<' not supported between instances of 'list' and 'int'
```
You can fix it by removing the `pad_token_id` item in HuggingFace cache `config.json` (e.g., the path may be like `~/.cache/huggingface/hub/models--LLM-PBE--LLMPC-Blue-Team-Llama3.1-8b-instruct/snapshots/xxx/config.json`) and run again.

## Contact
If you have any question, please contact <llmpc2024.info@gmail.com>.