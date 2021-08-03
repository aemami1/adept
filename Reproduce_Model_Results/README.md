# Code to reproduce results of Bert, RoBERTa, and DeBERTa (or any model on the HuggingFace Hub--https://huggingface.co/models--) on ADEPT

## Details of Code

Based on the script [`run_glue.py`](https://github.com/huggingface/transformers/blob/master/examples/text-classification/run_glue.py).

## Usage:

There are 4 overall setups for our task, 5-class (mod5), 5-class no context (mod5nocontext), 3-class (mod3), and 3-class no context (mod3nocontext).

Here is how to run the script on one of them, e.g. BERT-Large

```bash
export TASK_NAME=mod5

python evaluate_on_adept.py \
  --model_name_or_path bert-large-uncased \
  --task_name $TASK_NAME \
  --do_train \
  --do_eval \
  --max_seq_length 128 \
  --per_device_train_batch_size 32 \
  --learning_rate 2e-5 \
  --num_train_epochs 3 \
  --output_dir /tmp/$TASK_NAME/
```

