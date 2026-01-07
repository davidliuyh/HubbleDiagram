# Hubble Diagram
## 依赖库
按照requirements.txt安装对应python包即可

## 文件组织
1. data中存放用到的数据
   1. DES-Dovekie_HD.csv是拿来绘制Hubble diagram所使用的SN
   2. .FITS文件是超新星完整数据
   3. STAT+SYS.npz是Dovekie给出的covariance
   4. survey-bricks.fits是Legacy Survey给出的bricks，可以查询某个RA DEC下天体处在哪个brick里面
2. fits中存放了模型拟合结果
3. host_galaxy存放了20.0 arcsec半径搜索的距离SN最近的星系（注意不一定是宿主星系）
4. tractor_data是临时文件，搜索最近星系时要下载LSC的数据

## hubble.ipynb
### Read Data
按照DES-Dovekie_HD.csv的顺序整理所使用的SN的完整信息为SN_sample表格，顺序用ID表示。注意此顺序不能乱改因为STAT+SYS.npz储存的covariance是按照这个排序的。

### Hubble Diagram
绘制了一下Hubble Diagram

### Fit Cosmology
用Ultranest拟合不同宇宙学。目前Flat w0waCDM还有点问题。

### Crossmatch
#### Match LSC Bricks
为SN_sample中所有SN找到对应的LSC brick并记录在'SKY'和'BRICKNAME'列中。

#### Find Nearest Galaxy for a SN
按照20.0 arcsec半径搜索最近的galaxy，得到这个galaxy的信息为一个table，并存在host_galaxy中。这个tabled的'ID'对应SN_sample中的'ID', 'type'为源（星系）的类型，'dist'为到对应SN的距离.fits文件后缀为ID。已经完成，不需要再跑一遍了。

#### Find Host Galaxy for a SN
认为dist < 1.0 arcsec的是宿主星系，只找到1196个，老师说不用每个都找到。最后得到valid_host_galaxies这个list，每个元素为一个host galaxy的信息table，用它进行后续处理。