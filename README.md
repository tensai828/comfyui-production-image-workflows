# ComfyUI Workflows

A comprehensive collection of production-ready ComfyUI workflows for Stable Diffusion image generation, covering basic generation through advanced techniques.

## Overview

This repository contains meticulously documented workflows demonstrating best practices for using ComfyUI, a node-based interface for Stable Diffusion. Whether you're generating images from text prompts, applying AI-powered image transformations, or exploring advanced composition techniques, you'll find practical examples and detailed explanations.

## Features

- **Beginner-Friendly**: Start with foundational workflows for SD1.5 and SDXL
- **Advanced Techniques**: Explore ControlNets, image conditioning, and creative composition
- **Comprehensive Documentation**: Each section includes detailed README files with explanations and best practices
- **Tested Workflows**: All JSON files are production-ready and tested
- **Experiments**: Dedicated experimental workflows for comparing techniques and pushing boundaries

## Quick Start

1. Install [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
2. Clone this repository
3. Copy workflows to your ComfyUI installation
4. Load a workflow JSON file in ComfyUI
5. Ensure required models are installed (see section READMEs for specific requirements)

## Directory Structure

### 📚 [Basic](/basic/)
Foundation workflows for text-to-image generation:
- **Basic SD1.5 workflows**: Simple to advanced text-to-image pipelines
- **SDXL workflows**: Base model, refined, and advanced configurations
- **Advanced techniques**: LoRA application, CLIP skip, batching, and model comparison

Key concepts covered:
- Checkpoint loading and model selection
- Latent vs. pixel space understanding
- KSampler configuration (seed, steps, CFG, sampler/scheduler selection)
- VAE encoding/decoding
- LoRA (Low-Rank Adaptation) integration

### 🎨 [Text-to-Image](/text2img/)
Text prompt optimization and conditioning techniques:
- **Word Weighting**: Emphasis control using parentheses notation
- **Embeddings**: Textual inversion for custom styles
- **Conditioning Methods**: Concat, average, and area-based text conditioning
- **Advanced Techniques**: Timestepping and GLIGEN box for spatial text control

Experiments include conditioning method comparisons and creative prompt engineering.

### 📷 [Image-to-Image Conditioning](/image_conditioning/)
Reference-based generation and style transfer:
- **Simple Img2Img**: Denoise-controlled image variation
- **unCLIP Models**: Style-based generation from reference images
- **IPAdapter**: Image-based conditioning with SDXL
- **Style Models**: Temporal style transfer techniques
- **SDXL Revision**: Advanced SDXL-specific conditioning

Includes techniques for working with different image sizes and aspect ratios.

### 🎭 [Guided Composition](/guided_composition/)
ControlNet-based precise image generation:
- **Pose Control**: Character posing with OpenPose
- **Canny Edge Detection**: Sketch-based composition
- **Depth Maps**: 3D spatial control and volume definition
- **Multiple ControlNets**: Combining multiple control sources

Requires: [ComfyUI ControlNet Auxiliary Preprocessors](https://github.com/Fannovel16/comfyui_controlnet_aux)

### 🎨 [In/Out Painting](/in-out_painting/)
Image editing and expansion workflows:
- **Inpainting**: Precise region editing and modification
- **Outpainting**: Seamless image expansion
- **Cross-Model Inpainting**: Using SD1.5 inpainting with SDXL refiner
- **Seam Removal**: Post-processing techniques for seamless results

Best practices for mask creation, mask growing, and denoise optimization.

### 📈 [Upscaling](/upscale/)
Image enhancement and resolution optimization:
- **Latent Upscaling**: GPU-efficient upscaling in latent space
- **Pixel Upscaling**: Traditional and model-based upscaling
- **Resolution Enhancement**: Hires fix techniques and denoise configuration
- **Tile ControlNet**: Advanced upscaling with structural guidance

Comprehensive analysis of denoise impact and resource optimization.

## Requirements

### Core
- ComfyUI (latest version recommended)
- Python 3.10+
- GPU with sufficient VRAM (8GB minimum, 12GB+ recommended)

### Models
Different workflows require different checkpoints:
- **SD1.5**: v1-5-pruned-emaonly.safetensors
- **SDXL**: SDXL 1.0 base and refiner models
- **ControlNets**: Version-matched to your checkpoint (v1.5, v2.1, or SDXL)
- **Inpainting Models**: Specific inpainting checkpoints for optimal results

Download models from:
- [Hugging Face](https://huggingface.co/)
- [Civitai](https://civitai.com/)

### Extensions
Some workflows require additional extensions:
- [ComfyUI ControlNet Auxiliary Preprocessors](https://github.com/Fannovel16/comfyui_controlnet_aux) - For guided composition workflows

## Key Concepts

### Latent vs. Pixel Space
Stable Diffusion processes images in "latent space" — a compressed representation that's computationally efficient. Only the final output is converted to pixels. Understanding this distinction is crucial for efficient upscaling and generation.

### Denoise Parameter
Controls how much the model modifies an image:
- **Lower values (0.25-0.5)**: Minimal changes, preserves structure
- **Higher values (0.55+)**: Significant modifications, better details
- For latent upscaling: typically requires higher denoise (0.55+)

### Sampler & Scheduler
Different samplers achieve quality results at different step counts:
- **Recommended**: Euler Ancestral, DPM++ 2M Karras
- Experimentation is key — no universal "best" sampler

### CFG Scale
Classifier-Free Guidance controls prompt adherence:
- **Lower (4-5)**: Model has creative freedom
- **Higher (7-9)**: Stricter prompt following
- Typical range: 4-9 depending on checkpoint and prompt complexity

## Workflow Structure

Each workflow follows this typical pattern:

1. **Load Model**: Checkpoint loading (SD1.5, SDXL, or specialized models)
2. **Set Parameters**: Resolution, seed, steps, sampler configuration
3. **Text Encoding**: CLIP models process text prompts
4. **Sampling**: KSampler processes latent data iteratively
5. **Decoding**: VAE converts latent to pixel image
6. **Post-Processing**: Optional upscaling, refinement, or additional passes

## Tips & Best Practices

- **Color-Coded Connections**: ComfyUI uses colored dots on nodes to indicate connection types — match colors when building workflows
- **Experiment**: Workflow results are highly dependent on checkpoint, prompt, and parameters. Use the experimental workflows to compare approaches
- **Batch Processing**: Process multiple images simultaneously for efficiency
- **Save Frequently**: ComfyUI autosaves, but manual saves before major changes are recommended
- **Memory Management**: For large batches or high resolutions, adjust batch sizes to fit your GPU

## Model Compatibility Notes

⚠️ **Important**: ControlNets must match your checkpoint version:
- SD v1.5 ControlNets with SD v1.5 checkpoints
- SDXL ControlNets with SDXL checkpoints

Mixing versions will produce poor results.

## Random Number Generation

ComfyUI's random number generation differs from other UIs (A1111, InvokeAI, etc.), making it difficult to exactly reproduce images generated on other platforms. Use consistent seeds within ComfyUI workflows for reproducibility.

## Example Workflows by Use Case

| Use Case | Workflow | Location |
|----------|----------|----------|
| First-time generation | [Basic SD1.5](./basic/basic_workflow.json) | basic/ |
| Multiple images | [Batch Processing](./basic/basic_latent_batch.json) | basic/ |
| Custom styles | [LoRA Application](./basic/basic_lora.json) | basic/ |
| Precise composition | [Canny ControlNet](./guided_composition/canny.json) | guided_composition/ |
| Image improvement | [Img2Img](./image_conditioning/img2img_SDXL.json) | image_conditioning/ |
| Upscaling | [Latent Upscale](./upscale/upscale_latent.json) | upscale/ |
| Detail editing | [Inpainting](./in-out_painting/inpaint.json) | in-out_painting/ |
| Image expansion | [Outpainting](./in-out_painting/outpaint.json) | in-out_painting/ |

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Feel free to submit:
- New workflow examples
- Improvements to existing workflows
- Documentation enhancements
- Bug reports or suggestions

## Resources

### Learning
- [ComfyUI Documentation](https://github.com/comfyanonymous/ComfyUI)
- [Stable Diffusion Paper](https://arxiv.org/abs/2112.10752)
- [ControlNet Paper](https://arxiv.org/abs/2302.05543)

### Models & Assets
- [Civitai](https://civitai.com/) - Community models
- [Hugging Face](https://huggingface.co/) - Official models
- [PoseMy.Art](https://posemy.art/) - Character posing
- [Cascadeur](https://cascadeur.com/) - Professional character animation

## Troubleshooting

**Out of Memory (OOM)**
- Reduce batch size
- Lower latent resolution
- Use memory-optimized samplers
- Enable memory pooling in ComfyUI settings

**Poor Quality Results**
- Increase sampling steps (15-20 minimum)
- Adjust CFG scale (4-9 range)
- Try different samplers
- Use appropriate checkpoint for your task

**Seams in Outpainting/Inpainting**
- Increase mask grow value
- Use lower denoise for refinement passes
- Apply upscaling as final step

## Changelog

See individual workflow directories for detailed update information.

---

**Last Updated**: February 2026  
**ComfyUI Version**: Compatible with latest versions  
**Status**: Active development and maintenance
