# Example: Use This Webapp with Multiple Endpoints

To use this webapp with multiple MAX Object Detector endpoints, follow the steps below:

1. Build the MAX Object Detector with different underlying models (Run the following command in the MAX Object Detector source directory):

    docker build --build-arg model=ssd_mobilenet_v1 -t max-object-detector-mobilenet .
    docker build --build-arg model=faster_rcnn_resnet101 -t max-object-detector-rcnn .

2. Build the webapp (Run the following command in the webapp source directory):

    docker build -t max-object-detector-webapp .

3. Create a Docker network that will connect the to-be-containers:

    docker network create object-detector

4. Start the object detectors:

    docker run --rm -it --net=object-detector -p 5001:5000 --name=max-object-detector-mobilenet max-object-detector-mobilenet
    docker run --rm -it --net=object-detector -p 5002:5000 --name=max-object-detector-rcnn max-object-detector-rcnn

5. Start the webapp:

    docker run --rm -it -p 8080:8080 --net='object-detector' --env=endpoints='--model=http://max-object-detector-mobilenet:5000 --model=http://max-object-detector-rcnn:5000' max-object-detector-webapp

6. Visit `http://localhost:8080` in your browser and enjoy!
