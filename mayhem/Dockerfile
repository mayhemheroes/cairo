# Copyright 2018 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################
FROM gcr.io/oss-fuzz-base/base-builder
RUN apt-get update && apt-get install -y python3-pip gtk-doc-tools libffi-dev autotools-dev libtool gperf
RUN pip3 install -U meson==0.60.0 ninja

RUN git clone --depth 1 git://git.sv.nongnu.org/freetype/freetype2.git
RUN git clone --depth 1 https://gitlab.gnome.org/GNOME/glib
COPY . cairo
RUN rm -rf cairo/mayhem

ADD https://raw.githubusercontent.com/google/fuzzing/master/dictionaries/png.dict $SRC/cairo.dict

WORKDIR $SRC/cairo
COPY mayhem/targets $SRC/fuzz
COPY mayhem/build.sh $SRC/

ENV FUZZING_LANGUAGE=c SANITIZER=address
RUN compile
RUN chmod +x /out/pdf_surface_fuzzer
