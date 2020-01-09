<template>
    <div>
        <el-dialog
                title="提示"
                :visible.sync="dialogVisible"
                width="30%">
            <span>这是一段信息</span>
            <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
  </span>
        </el-dialog>


        <h1>音乐专辑管理</h1>

        <el-table
                :data="albums"
                style="width: 80%">
            <el-table-column
                    prop="album_id"
                    label="ID"
                    width="100">
            </el-table-column>
            <el-table-column
                    prop="album_name"
                    label="专辑名"
                    width="200">
            </el-table-column>
            <el-table-column
                    prop="price"
                    label="价格">
            </el-table-column>
            <el-table-column
                    prop="singers"
                    label="歌手名">
            </el-table-column>
            <el-table-column
                    fixed="right"
                    label="操作"
                    width="100">
                <template slot-scope="album">
                    <el-button @click="dialogVisible" type="text" size="small">详细内容</el-button>
                    <el-button @click="dialogVisible=true" type="text" size="small">编辑</el-button>
                    <el-button @click="dialogVisible=true" type="text" size="small">追加歌手</el-button>
                    <el-button @click="deleteBook(album)" type="text" size="small">删除</el-button>
                </template>
            </el-table-column>

        </el-table>
<!--        <h2>总价格： {{priceTotal}}</h2>-->

    </div>
</template>

<script>
    export default {
        name: "AlbumManger",
        data(){
            return {
                maxId:2,
                album:{album_id:'', album_name:'',price:'', singers:''},
                dialogVisible:false,
                albums:[],
                albumsUrl:"http://localhost:3000/album"
            }
        },
        created() {
            console.log("[INFO] Begin created")
            fetch(this.albumsUrl)
                .then(res => res.json())
                .then(bs => this.albums = bs)
            console.log("[INFO] Albums init data"+this.albums)
        },
        methods:{
            deleteAlbum(album){
                console.log("[INFO] albums info "+ album)
                fetch(this.albumsUrl+"/"+album.album_id,{method:"DELETE"})
                    .then(res=>res.json())
                    .then(()=>{
                        let index = this.albums.findIndex(item => item.album_id == album.album_id)
                        this.albums.splice(index, 1)
                    })
            },
            addAlbum(){
                fetch(this.albumsUrl, {
                    method: "POST", headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify(this.album)
                }).then(res=>res.json())
                    .then(nb=>this.album.push(nb))
            }
        }

    }
</script>

<style scoped>

</style>