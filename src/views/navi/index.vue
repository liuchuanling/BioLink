<script lang="ts"></script>
<template>
  <div class="navi-page">
    <div class="site-list">
      <div v-for="site in sites" :key="site.name" class="site-card" @click="openSite(site.url)">
        <img :src="site.img" :alt="site.name" class="site-img" />
        <div class="site-info">
          <h2>{{ site.name }}</h2>
          <p>{{ site.desc }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import NcbiLogo from "@/assets/navi/NcbiLogo.png";
import PubMedLogo from "@/assets/navi/PubMedLogo.png";
import NatureLogo from "@/assets/navi/NatureLogo.svg";
import ScienceLogo from "@/assets/navi/ScienceLogo.svg";
import CellPressLogo from "@/assets/navi/CellPressLogo.svg";
import PdbLogo from "@/assets/navi/PdbLogo.png";
import EnsemblLogo from "@/assets/navi/EnsemblLogo.png";
import WebOfScienceLogo from "@/assets/navi/WebOfScienceLogo.svg";

interface Site {
  name: string;
  url: string;
  img: string;
  desc: string;
}

export default defineComponent({
  setup() {
    const sites = ref<Site[]>([
      {
        name: "NCBI",
        url: "https://www.ncbi.nlm.nih.gov/",
        img: NcbiLogo,
        desc: "美国国家生物技术信息中心，提供基因组、文献、数据库等生物信息资源。"
      },
      {
        name: "PubMed",
        url: "https://pubmed.ncbi.nlm.nih.gov/",
        img: PubMedLogo,
        desc: "全球权威的生物医学文献检索数据库。"
      },
      {
        name: "Nature",
        url: "https://www.nature.com/",
        img: NatureLogo,
        desc: "国际著名的综合性科学期刊，涵盖生命科学等多个领域。"
      },
      {
        name: "Science",
        url: "https://www.sciencemag.org/",
        img: ScienceLogo,
        desc: "美国科学促进会主办的顶级科学期刊，发布最新科研成果。"
      },
      {
        name: "Cell",
        url: "https://www.cell.com/",
        img: CellPressLogo,
        desc: "生命科学领域权威期刊，聚焦细胞生物学等前沿研究。"
      },
      {
        name: "Protein Data Bank (PDB)",
        url: "https://www.rcsb.org/",
        img: PdbLogo,
        desc: "蛋白质结构数据库，提供蛋白质三维结构信息。"
      },
      {
        name: "Ensembl",
        url: "https://www.ensembl.org/",
        img: EnsemblLogo,
        desc: "欧洲生物信息研究所提供的基因组浏览与注释平台。"
      },
      {
        name: "Google Scholar",
        url: "https://scholar.google.com/",
        img: "https://scholar.google.com/intl/en/scholar/images/1x/scholar_logo_64dp.png",
        desc: "谷歌学术搜索，全球学术文献检索工具。"
      },
      {
        name: "Web of Science",
        url: "https://www.webofscience.com/",
        img: WebOfScienceLogo,
        desc: "全球权威的科研文献与引文数据库。"
      },
      {
        name: "bioRxiv",
        url: "https://www.biorxiv.org/",
        img: "https://www.biorxiv.org/sites/default/files/biorxiv_logo_homepage.png",
        desc: "生命科学领域的预印本论文平台。"
      }
    ]);

    const openSite = (url: string) => {
      window.open(url, "_blank");
    };

    return { sites, openSite };
  }
});
</script>

<style scoped>
.navi-page {
  padding: 24px;
}
.site-list {
  display: flex;
  flex-wrap: wrap;
  gap: 24px;
}
.site-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 260px;
  padding: 16px;
  cursor: pointer;
  background: #fafbfc;
  border: 1px solid #eeeeee;
  border-radius: 8px;
  transition: box-shadow 0.2s;
}
.site-card:hover {
  box-shadow: 0 2px 12px rgb(0 0 0 / 8%);
}
.site-img {
  width: 80px;
  height: 80px;
  margin-bottom: 12px;
  object-fit: contain;
  background: #ffffff;
  border-radius: 4px;
}
.site-info h2 {
  margin: 0 0 8px;
  font-size: 1.1em;
}
.site-info p {
  margin: 0;
  font-size: 0.95em;
  color: #555555;
}
</style>
