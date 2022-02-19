## What is it?

Background Calibration analyzes the empty nozzle tip and background in the bottom camera. The obtained baseline information is later used to remove the background in [[Bottom Vision]]. The Background Calibration is automatically (and repeatedly) performed during the [[Nozzle Tip Calibration|Nozzle-Tip-Calibration-Setup]]. 

## Calibration Operating Principle

The new Background Calibration reuses the images that are already captured by the [[Nozzle Tip Calibration|Nozzle-Tip-Calibration-Setup]], so no extra machine time and motion is needed to obtain them. It analyzes these background images for the **Key Color** (Chroma Key) of the Juki (or similar) nozzle tip. Any dominant, vivid color is automatically detected and a likely bounding box computed in the [HSV Color Model](https://en.wikipedia.org/wiki/HSL_and_HSV). 

Furthermore, a cutoff brightness is determined where the non-color-keyed parts of the background must all be darker. This indicates the minimum brightness applicable typically in a `Threshold` stage (the subject of an upcoming PR). 

The background calibration automatically blots out the center piece in the calibration image, where the nozzle tip was detected. This will eliminate any shiny elements that are typically present (needles, scraped off points etc.). OpenPnP assumes this center part to be always covered by the picked part, i.e. there is no need to include its color in the background calibration, _nor_ these shiny elements treated as a problem (see Trouble Shooting). The blotted-out disc has a size of **Vision Diameter** + 2 x **Minimum Detail Size** (see Configuration).

The resulting calibration is also validated, and if the detected quality is not good, diagnostic guidance and visual feed-back is provided for users to remedy the situation (see section Trouble Shooting below).

## Calibration Pipeline Control

If the Background Calibration is enabled, the following is controlled in the bottom vision pipeline:

1. The `BlurGaussian` `kernelSize` property is controlled by a new **Minimum Detail Size** field, where the user can enter a length (in system units) that indicates the smallest sensible size of a feature on a part (such as the dimension of a pad, pin, ball of a package). The GaussianBlur is configured accordingly, to suppress image noise, dirt, texture etc. that is not relevant for detection. This is also applied to the Background Calibration itself, to get the same `MaskHSV` input data as the pipeline.
2. The `MaskCircle` `diameter` property is controlled by the already existing **Max. Part Diameter** field on the Nozzle tip. Peripheral background content that might disrupt the vision operation is masked out. This is also applied to the Background Calibration itself, to get the same `MaskHSV` input data as the pipeline.
3. The `MaskHSV` `hueMin`, `hueMax`, `saturationMin`,  `saturationMax`, `valueMin` and `valueMax` properties are all controlled by the Background Calibration bounding box.  

# Instructions for Use

## Configuration
There is a new Background Calibration section on the Calibration tab: 

![Screenshot_20220219_154325](https://user-images.githubusercontent.com/9963310/154806475-db1c38d9-7be9-44f7-8bd4-86e06fbdcba4.png)

**Method** controls the Background Calibration:

![Screenshot_20220219_160944](https://user-images.githubusercontent.com/9963310/154806671-75d40a06-bd88-42d1-b309-517f283cc75a.png)

- **None** switches Background Calibration off. The pipeline is no longer controlled, i.e. the user can edit the properties in the Pipeline Editor. 
- **Brightness** only determines the cutoff brightness of the nozzle tip and background, no key color is assumed to be present. 
- **BrightnessAndKeyColor** determines both the nozzle tip key color, and the remaining background cutoff brightness. 

**Minimum Detail Size** (in system units) configures the smallest valid detail of a part to be detected in bottom vision, such as the smallest dimension of a pad, pin, ball of a package. All smaller image details like image noise, textures, dirt etc. are considered irrelevant for detection. The **Minimum Detail Size** controls a blurring filter to suppress these artifacts.

**Minimum** and **Maximum** columns in the **Hue** (base color), **Saturation**, **Value** (Brightness) [HSV color model](https://en.wikipedia.org/wiki/HSL_and_HSV) indicate the calibrated bounding box of the key color (if enabled). 

The HSV color model can be illustrated like this (Wikipedia):

![image](https://user-images.githubusercontent.com/9963310/154807032-f8de9cd1-daf9-4de3-8fb2-02884266c4e7.png)

**Tolerance** can be used for each channel, to expand the detected bounding box. This provides robustness against changing ambient lighting, shadows cast by the picked parts, etc. Note: practical tolerance values are yet to be determined in testing. The tolerance is applied to both sides of the channel bounds, except for Saturation Maximum, and Value Minimum which are both  left unlimited in the bottom vision `MaskHSV` stage. OpenPnP assumes both darker and more vivid (greener) pixels are always part of the background, even if they were not detected, or cut off in the calibration. 

## Application

The Background Calibration data is then used to automatically parametrize the bottom vision operation. If the Background Calibration **Method** is not **None**, the relevant stage properties in the pipeline are fully controlled. If the background was successfully calibrated, the knockout should now be perfect:

![nozzle-tip-background-knockout](https://user-images.githubusercontent.com/9963310/154805842-e565ec69-a890-4757-963c-2ffe603cdf3f.gif)

As the Nozzle Tip Calibration is typically renewed at least whenever the machine was homed, or even with each nozzle tip change, the calibration data should be adaptive to changing ambient light and other conditions. A manual recalibration can also always be triggered, if needed.

Furthermore, any (color) differences between nozzle tips are also implicitly handled. This is important, when packages are compatible with multiple such nozzle tips. The pipeline (with is part/package dependent, but not nozzle tip dependent), can therefore dynamically react to these differences, through the calibration control. 

## Trouble Shooting

The calibration provides diagnostic feed-back regarding the quality of the background. A text describes the problem(s) and provides rough guidance as to how the problem can be remedied. Furthermore, the button **Show Problems** can be pressed to display the problematic image portions in the camera preview:

![nozzle-tip-background-diagnostics](https://user-images.githubusercontent.com/9963310/154807616-560c3710-6de2-4c44-ae6e-52625f0b4041.gif)

Credits: Nozzle tip images by Jan, with thanks!