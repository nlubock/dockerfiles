# Start with CUDA Theano base image
FROM nlubock/cuda-theano

# Install dependencies
RUN apt-get update && apt-get install -y \
  libhdf5-dev \
  python-h5py \
  python-yaml

# Upgrade six
RUN pip install --upgrade six

# Clone Keras repo and move into it
RUN cd /root && git clone https://github.com/fchollet/keras.git && cd keras && \
  # Install
  python setup.py install

# set up keras to use theano backend
RUN mkdir /root/.keras && \
   echo "{\n\t\"image_dim_ordering\": \"th\",\n\t\"epsilon\": 1e-07,\n\t\"floatx\": \"float32\",\n\t\"backend\": \"theano\"\n}" > /root/.keras/keras.json

# Set WD to home
WORKDIR /root
