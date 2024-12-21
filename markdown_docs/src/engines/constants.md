## FunctionDef _build_neural_engine(model_name, checkpoint_step)
**_build_neural_engine**: This function constructs a neural engine based on the specified model name and checkpoint step. It configures the transformer model parameters according to predefined settings for different models, loads the appropriate checkpoint, and returns an instance of the neural engine tailored to the policy used by the model.

**parameters**:
· model_name: A string representing the name of the model to be built. Valid options include '9M', '136M', '270M', and 'local'.
· checkpoint_step: An integer indicating the step number of the checkpoint to load. Defaults to -1, which may imply loading the latest or a default checkpoint.

**Code Description**: The function starts by determining the model's configuration based on the `model_name` parameter using pattern matching. It sets parameters such as policy type, number of layers, embedding dimension, and number of heads for different models. For unknown model names, it raises a ValueError.

Next, it defines the output size based on the policy. If the policy is 'action_value' or 'state_value', the output size is set to `num_return_buckets`. If the policy is 'behavioral_cloning', the output size is set to the number of actions defined in the utils module.

A `predictor_config` object is created using these parameters, specifying the configuration for a transformer model. This includes settings like vocabulary size, positional encodings type, sequence length, and more.

The function then builds a predictor using this configuration with `transformer.build_transformer_predictor`. It constructs the checkpoint directory path based on the current working directory and the model name.

Parameters are loaded from the specified checkpoint directory using `training_utils.load_parameters`, which takes the initial parameters of the predictor as input. The step parameter allows specifying which checkpoint to load, defaulting to -1 if not provided.

The function retrieves return bucket values for uniform distribution using `utils.get_uniform_buckets_edges_values`. Finally, it returns an instance of a neural engine from the policy specified in the configuration. This is achieved by wrapping the predictor's predict function with additional parameters and passing it along with the return bucket values to the appropriate engine constructor from `neural_engines.ENGINE_FROM_POLICY`.

**Note**: The function assumes that certain modules (`transformer`, `utils`, `training_utils`, `tokenizer`, `jrandom`, `np`, `os`, `neural_engines`) are properly imported and available in the environment. It also relies on predefined constants such as `NUM_ACTIONS` and `SEQUENCE_LENGTH`.

**Output Example**: Depending on the model name and checkpoint step, the function returns an instance of a neural engine configured for the specified policy. For example, if '9M' is passed as the model_name with a valid checkpoint_step, it might return an action_value-based neural engine with 8 layers, an embedding dimension of 256, and 8 heads, ready to perform predictions using the loaded parameters from the checkpoint.
