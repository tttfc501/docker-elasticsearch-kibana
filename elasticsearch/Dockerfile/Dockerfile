ARG elasticsearch_version=7.4.2
FROM elasticsearch:${elasticsearch_version}

#ARG ik_plugin_url=https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.4.2/elasticsearch-analysis-ik-7.4.2.zip
#RUN /usr/share/elasticsearch/bin/elasticsearch-plugin install -b ${ik_plugin_url}
#ARG pingyin_plugin_url=https://github.com/medcl/elasticsearch-analysis-pinyin/releases/download/v7.4.2/elasticsearch-analysis-pinyin-7.4.2.zip
#RUN /usr/share/elasticsearch/bin/elasticsearch-plugin install ${pingyin_plugin_url}

ARG ik_plugin_url=elasticsearch-analysis-ik-7.4.2.zip
ARG pingyin_plugin_url=elasticsearch-analysis-pinyin-7.4.2.zip
COPY plugins /usr/share/plugins
RUN /usr/share/elasticsearch/bin/elasticsearch-plugin install -b file:///usr/share/plugins/${ik_plugin_url}
RUN /usr/share/elasticsearch/bin/elasticsearch-plugin install file:///usr/share/plugins/${pingyin_plugin_url}

ARG ik_custom_dict=http://test.dict.com/custom.dic
ARG ik_custom_stopword_dict=http://test.dict.com/custom_stopwords.dic
RUN sed -i "/remote_ext_dict/c <entry key=\"remote_ext_dict\">${ik_custom_dict}</entry>" /usr/share/elasticsearch/config/analysis-ik/IKAnalyzer.cfg.xml && sed -i "/remote_ext_stopwords/c <entry key=\"remote_ext_stopwords\">${ik_custom_stopword_dict}</entry>" /usr/share/elasticsearch/config/analysis-ik/IKAnalyzer.cfg.xml

WORKDIR /usr/share/elasticsearch
