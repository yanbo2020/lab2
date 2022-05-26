# lab2-

from flask import Blueprint, jsonify, request
from UseSqlite import RiskQuery
from upload_bp import make_html_paragraph
from show_bp import get_database_photos
import json
import collections
import os

api_bp = Blueprint('api_bp', __name__)
data = []


@api_bp.route('/api/json', methods=['GET'])
def api_response():
    if request.method == 'GET':
        rq = RiskQuery('.\\static\\RiskDB.db')
        rq.instructions("SELECT * FROM photo ORDER By time desc")
        rq.do()
        data = rq.results.fetchall()

        objects_list = []
        i = 1
        for row in data:
            d = collections.OrderedDict()
            d["id"] = i
            d["upload_date"] = row[0]
            d["photo_size"] = os.path.getsize(row[2])/1024
            d["description"] = row[1]
            objects_list.append(d)
            i+=1
        
        j = json.dumps(objects_list)
        
        return j



