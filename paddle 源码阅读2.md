//this = scaleAB*(a*b) + scaleT*this
Matrix::mul(const MatrixPtr a, const MatrixPtr b, real scaleAB, real scaleT)

以下更新weight的梯度：WGrad = input_T * oGrad + WGrad
weights_[i]->getWGrad()->mul(input_T, oGrad, 1, 1);

令u = input_T，v = oGrad，将（u，v）放入local_sv_queues_

借鉴poseidon的comm bus在worker之间建立p2p连接。

worker的id从0到n

第i个worker和第0到i-1个worker建立连接，并发送worker_id

第i个worker会接到n - i - 1个连接，和i个确认

这样就建立起p2p连接

