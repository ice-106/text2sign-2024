# Overview Steps

1. Data preperation
2. Model Training
3. Visualize the model

## Data Preperation: `scripts/data_preparation.py`

1. Reduce Framerates
2. Trim Video
3. Extract Skeletons by MediaPipe: `scripts/convert_vdo_to_skeletons.py` then got `.npy` as an output
4. Normalize then get `.skels`

NOTE: .skels is be created in `scripts/norm_standardize.py`

see `scripts/data_preparation.py` for all the flow

## Visualize the model

The video are genereated in this file `src/capstone_utils/plot_all_body.py` (needs .skels fil (train.skels))
