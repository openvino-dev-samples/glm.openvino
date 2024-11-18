English | [简体中文](README_zh.md)

# glm.openvino Demo

Here is an example of how to deploy ChatGLM3 using OpenVINO

## 1. Environment configuration

We recommend that you create a new virtual environment and then install the dependencies as follows. The 
recommended Python version is `3.10+`.

Linux

```
python3 -m venv openvino_env

source openvino_env/bin/activate

python3 -m pip install --upgrade pip

pip install wheel setuptools

pip install -r requirements.txt
```

Windows Powershell

```
python3 -m venv openvino_env

.\openvino_env\Scripts\activate

python3 -m pip install --upgrade pip

pip install wheel setuptools

pip install -r requirements.txt
```

## 2. Convert model

Since the Huggingface model needs to be converted to an OpenVINO IR model, you need to download the model and convert.

```
python3 convert.py --model_id ZhipuAI/chatglm3-6b --precision int4 --output {your_path}/chatglm3-6b-ov --modelscope
```

### Parameters that can be selected

* `--model_id` - path (absolute path) to be used from Huggngface_hub (https://huggingface.co/models) or the directory
  where the model is located.
* `--precision` - model precision: fp16, int8 or int4.
* `--output` - the path where the converted model is saved
* `--modelscope` - if downloading the model from Model Scope.

## 3. Run the streaming chatbot

```
python3 chat.py --model_path {your_path}/chatglm3-6b-ov --max_sequence_length 4096 --device CPU
```

or

```
python3 chat_genai.py --model_path {your_path}/chatglm3-6b-ov --max_sequence_length 4096 --device CPU
```

### Parameters that can be selected

* `--model_path` - The path to the directory where the OpenVINO IR model is located.
* `--max_sequence_length` - Maximum size of output tokens.
* `--device` - The device to run inference on. e.g "CPU","GPU".

## example

```
User: Hello
AI Assistant: Hello! Is there anything I can do to help you?

User: Who are you?
ChatGLM3-6B-OpenVINO: I am an artificial intelligence assistant named ChatGLM3-6B, which was developed from a language model jointly trained by Tsinghua University’s KEG Laboratory and Zhipu AI Company in 2023. My role is to provide appropriate responses and support to users' questions and requests.

User: Please tell me an interesting story
ChatGLM3-6B-OpenVINO: Once upon a time, there was a little boy named Xiao Ming who loved animals very much. One day, while walking in the forest, he discovered a very beautiful bird. The bird was injured and could not fly. Xiao Ming felt very distressed and decided to take care of the little bird. Xiao Ming took the bird home, built a small nest for it, and found some soft grass and food. Every day, he feeds the bird and changes its water. Gradually, the bird's injury recovered and began to fly around Xiao Ming's home. They became very good friends. However, one day, Xiao Ming's parents told him that they must send Xiao Ming's little bird to the forest. Xiao Ming was very sad because he had become good friends with Xiaoniao. However, his parents told him that the bird would be more free in the forest and that he could continue to watch the bird's life in the forest. So, Xiao Ming and his parents sent the bird to the forest. The little bird is very happy because it can fly again and has many other little animal friends. Xiao Ming also felt very happy because he knew that even if he could not always have the birds, he could still appreciate their beauty in nature. From then on, Xiao Ming often came to the forest to look for birds.

User: Please give this story a title
ChatGLM3-6B-OpenVINO: "The Power of Friendship: Xiao Ming and Bird's Forest Adventure"
```

## Common problem

1. Why does a huggingface link error appear when importing a local model?
    - Downgrade transformers library to version 4.37.2

2. Do I need to install the OpenVINO C++ inference engine?
    - Unnecessary

3. Do I have to use Intel hardware?
    - We only tried it on Intel devices, and we recommend using x86 architecture Intel devices, including but not
      limited to:
    - Intel CPU, including personal computer CPU and server CPU.
    - Intel's integrated GPU. For example: Arc™ Series and Iris® Series.
    - Intel's discrete graphics card. For example: ARC™ A770 graphics card.
  
4. Why OpenVINO cannot find GPU device in my system?
   - Ensure OpenCL diivess are installed correctly.
   - Ensure you enabled the right permissions for GPU device
   - More information can be found in [Install GPU drivers](https://github.com/openvinotoolkit/openvino_notebooks/wiki/Ubuntu#1-install-python-git-and-gpu-drivers-optional)

5. Whether support C++?
   - Please refer to this [example](https://github.com/openvinotoolkit/openvino.genai/tree/master/src)
