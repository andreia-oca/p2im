version=0.1

build:
	docker image build -t p2im_img:${version} --build-arg VERSION=${version} .

test: build
	docker run --rm --name p2im_temp p2im_img:${version}

run: build
	docker run -it --name p2im p2im_img:${version} /bin/bash

bash:
	docker exec -it p2im /bin/bash

clean:
	docker rm -f p2im
	docker image rm p2im_img:${version}
