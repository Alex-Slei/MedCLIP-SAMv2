# Breast ultrasound prompt sensitivity

[prompt_library.csv](prompt_library.csv) contains 81 prompts: nine target terms across nine wording templates. The experiment measures how wording affects segmentation with the model and pipeline fixed.

## Run the prompts

1. Pilot the 12 rows marked `pilot=true` on a few validation images. Check that each prompt produces its own masks and scores. For the full validation run, reuse pilot results only where the images and settings match exactly.
2. Run all 81 prompts on the validation split. Apply each prompt to every image. For each prompt, create an image-ID-to-text JSON using the same key format as the released breast tumor prompt file, and pass it to the saliency step via `--json-path` with `--reproduce`. Keep the baseline checkpoints, seed, preprocessing, and segmentation settings fixed. Use separate output paths for each split and prompt. Leave the released prompt JSON unchanged.
3. Save one score row per image and prompt with `prompt_id,image_id,dice,nsd`. Record the split separately or add a `split` column. Use the same evaluator and NSD tolerance as the baseline, and check that every prompt has a score for every expected image.

## Compare the results

| Comparison | What it tests |
| --- | --- |
| Same batch: IDs ending in 01, 03, and 04 | Target terminology: tumor, lesion, and mass |
| Same batch: 04 vs. 05 | Clinical vs. everyday wording: mass vs. lump |
| Same batch: 01 vs. 02 | Spelling: tumor vs. tumour |
| C vs. D or E vs. F, matching term IDs | Adding the word ultrasound |
| F vs. H, matching term IDs | Ultrasound vs. sonographic wording |
| F vs. G or F vs. I, matching term IDs | Rephrasing with the target and modality held fixed |

Compare Dice and NSD on the same images. Report the average differences and inspect cases where wording helped or hurt. Terms such as abnormality and nodule can change the scope of the target, so treat those as terminology comparisons.

Choose one prompt by mean validation Dice and fix its wording before running it on the UDIAT test set. Compare it with A01 (`breast tumor`) as a simple reference. Report the released per-image prompts as a separate benchmark because they include benign or malignant subtype information.
