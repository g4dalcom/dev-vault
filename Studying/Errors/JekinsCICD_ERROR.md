# FreeSwapSpace0

- 호스트에 Swap 파일이 구성되어 있는지 확인
```shell
free|grep -i Swap
```
- Swap:            0          0          0
- 위와 같이 뜨면 구성이 되어있지 않은 것임

- Swap 파일 구성하기
```shell
sudo dd if=/dev/zero of=swapfile bs=1M count=1K
sudo mkswap swapfile
sudo chown root:root swapfile
sudo chmod 600 swapfile
sudo swapon swapfile

#스왑이 구성되었는지 확인
free|grep -i Swap

Swap:      1048572          0    1048572
```


# # "No space left on device"
###### 임시
`docker system prune`
`docker volume prune`

- 디스크 확인
`df -h`
`df -i` inode 공간 확인

- 찾기
`du sh *`
`du -h --max-depth=1` 해당 경로에서 바로 용량 확인
`du -hs * | sort -rh | head -5` 폴더별 용량 sort 해서 보기

- 로그파일 비우기
`cat /dev/null > /var/log/kern.log` kern.log 용량 0으로 바꾸기
`cat /dev/null > /var/log/syslog`  syslog 용량 0으로 바꾸기
`sudo rm -rf syslog.*` 나머지 삭제 등등

- 휴지통 비우기
`rm -rf ~/.local/share/Trash/files/*`